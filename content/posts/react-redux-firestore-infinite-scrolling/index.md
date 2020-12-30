+++
title = "React Redux Firestore Infinite Scrolling"
tags = ["react", "redux", "firestore"]
categories = ["Software-Dev"]
draft = true
+++

Recently I implemented infinite scrolling at [Artifai](https://artif.ai) with react-redux-firebase. It's not too difficult but it's helpful to work off of an example.

See below the component where the querying takes place.

```js
export default () => {
  const firestore = useFirestore();
  const [recentArtifaications, setRecentArtifaications] = React.useState([]);

  const loadMorePosts = () => {
    (async () => {
      let recentArtifaicationsQuery = firestore
        .collectionGroup("artifaications")
        .where("accessLevel", "==", "public")
        .where("percentProcessed", "==", 100)
          .orderBy("created", "desc").limit(15);

      recentArtifaications.length !== 0 &&
        recentArtifaicationsQuery.startAfter(
          recentArtifaications.slice(-1)[0]["created"]
        );

      let recentArtifaicationsSnapshot = [];
      const querySnapshot = await recentArtifaicationsQuery.get();

      querySnapshot.forEach((doc) => {
        recentArtifaicationsSnapshot.push({
          id: doc.id,
          ...doc.data(),
        });
      });

      setRecentArtifaications([
        ...recentArtifaications,
        ...recentArtifaicationsSnapshot,
      ]);
    })();
  };

  useEffect(() => {
    // call loadMorePosts on component mount
    loadMorePosts();
  }, []);

  return (
    <>
      <Typography variant="h2" style={{ marginTop: "150px" }} align="center">
        Explore Other's Creations
      </Typography>
      <TripleImageViewList
        feed={recentArtifaications}
        view="general"
        loadMorePosts={loadMorePosts}
      />
    </>
  );
};
```

The function `loadMorePosts` loads the next 15 posts from firestore. This method is called from `TripleImageViewList` seen below.

```js
const TripleImageViewList = (props) => {
  console.log(props.feed);
  return (
    <>
      <br />
      {props.feed.map((tileData, index) => (
        <React.Fragment key={tileData.id}>
          <TripleImageCard
            tileData={tileData}
            index={index}
            cardHeight={cardHeight}
            view={props.view}
          />
          {(index === props.feed.length - 5 && (props.feed.length < 100)) && (
            <Waypoint onEnter={() => props.loadMorePosts()} />
          )}
        </React.Fragment>
      ))}
    </>
  );
};
```

Here we use the [react-waypoint](https://github.com/civiccc/react-waypoint) library to trigger the `loadMorePosts` when the user only has 5 cards left to see. Ta-da! You now have infinite scrolling in your React app.

Links:

-   [Infinite Scrolling with React Waypoint](https://www.youtube.com/watch?v=9dRk3bxEbS8&t=371s)
-   [Infinite Scroll in React.js](https://stackoverflow.com/questions/60789004/about-infinite-scroll-in-react-js-and-material-ui)
