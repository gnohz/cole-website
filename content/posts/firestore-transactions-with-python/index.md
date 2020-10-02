+++
title = "Firestore Transactions with Python"
date = 2020-10-02T10:16:00-04:00
tags = ["firestore", "python", "firebase"]
categories = ["Software-Dev"]
draft = false
+++

The documentation on writing firestore transactions with python is not especially extensive, so I figured I'd share what I learned when setting up transactions for my project [Artifai](https://artif.ai).

My goal was to pull an item from a queue collection, but I needed to avoid the scenario where two threads pull items from the queue at the same time. Transactions are perfect for this because you can ensure that no two machines pull the same item off the queue.

> In the case of a concurrent edit, Cloud Firestore runs the entire transaction again. For example, if a transaction reads documents and another client modifies any of those documents, Cloud Firestore retries the transaction. This feature ensures that the transaction runs on up-to-date and consistent data.

Here is how you create a transaction. Define a method with the `@firestore.transactional` decorator that has parameters for a transaction and a query reference.

```python
@firestore.transactional
def claim_artifaication(transaction, queue_objects_ref):
    # query firestore
    queue_objects = queue_objects_ref.stream(transaction=transaction)

    # pull the document from the iterable
    next_item = None
    for doc in queue_objects:
        next_item = doc

    # if queue is empty return status code of 2
    if not next_item:
        return {"status": 2}


    # get information from the document
    next_item_data = next_item.to_dict()
    next_item_data["status"] = 0

    # delete the document and return the information
    transaction.delete(next_item.reference)
    return next_item_data
```

The goal of this transaction is to

1.  read the last document from the queue
2.  delete the document
3.  return the information the document was storing

If this transaction is in progress and the queue collection gets modified (by another thread pulling an item from the queue), it will restart the transaction; this ensures that no two threads will pull the same item off of the queue. If there are no items left on the queue, the method returns a dictionary with status set to 2 (to be handled later in the program).

Great! We have now defined a transaction. In order to execute it you can do the following:

```python
import firebase_admin
from firebase_admin import credentials, storage, firestore

db = firestore.client()
transaction = db.transaction()

# initialize query
queue_objects_ref = (
    db.collection("state")
    .document("artifaicationQueue")
    .collection("queueObjects")
    .order_by("created", direction="ASCENDING")
    .limit(1)
)

transaction_attempts = 0
while True:
    try:
        # apply transaction
        next_item_data = claim_artifaication(transaction, queue_objects_ref)
        logging.debug("Successfully applied transaction")
        break
    except Exception as e:
        logging.error(f"Could not apply transaction. Error: {e}")
        time.sleep(5)
        transaction_attempts += 1
        if transaction_attempts > 20:
            db.collection("errors").document(str(uuid.uuid4())).set({
                "exception": f"Could not apply artifaication claim transaction. Error: {e}",
                "location": "Claim artifaication",
                "time": str(datetime.now())
            })
            exit()
```

We create our `queue_objects_ref`, and then repeatedly try to execute the transaction in a `while True` loop. If the transaction fails, it throws an error which gets caught by the try except statement. If it isn't able to complete the transaction in 20 tries, it gives up and exits the program.

Hopefully this gives you an idea for how to build workflows with firestore transactions in python. Let me know if you have any comments or questions below!
