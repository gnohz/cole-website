---
title: "Querying for Most Recent Firestore Documents with React-Redux-Firebase"
date: 2020-06-10T14:09:18-04:00
draft: false
tags: ["firebase", "react", "redux"]
categories: ["Technical"]
---

# Objective

Sometimes you want to query for the most recent firestore documents in a certain collection. In my case I wanted to display all documents that were created less than 20 minutes ago. Here is how I did it.

# Prepping Firestore Documents

First, add a "created" field to the documents in the interested collection using firestore.Timestamp.now()

{{< highlight javascript >}}
this.props.firestore
  .collection("users")
  .doc(this.props.auth.uid)
  .collection("artifaications")
  .doc(artifaicationId)
  .set({
    contentImagePath,
    contentImageUrl: downloadUrls[0],
    styleImagePath,
    styleImageUrl: downloadUrls[1],
    artifaiImagePath,
    percentProcessed: 10,
    created: this.props.firestore.Timestamp.now()
  });
{{< /highlight >}}

# Syncing Redux Store with Firestore

Then add a useFirestoreConnect to the relevant component in order to keep the redux state in sync with firebase. Notice how we add the queryParams to order by value and take the first 10 documents.

Finally we use the javascript filter function to pull only those documents where the difference between the current time and created time is less than 20 minutes.

{{< highlight javascript >}}
const { artifaications, uid } = useSelector((state) => ({
  artifaications: state.firestore.ordered.artifaications || [],
  uid: state.firebase.auth.uid,
}));

useFirestoreConnect([
  {
    collection: `users/${uid}/artifaications`,
    queryParams: ["orderByValue", "limitToFirst=10"],
    storeAs: "artifaications",
  },
]);

artifaications.forEach((artifaication) => console.log(artifaication.created));

const recentArtifaications = artifaications.filter(artifaication => (Date.now() - artifaication.created.toDate())/(1000 * 60) < 20);
{{< /highlight >}}
