# Create Server Routes

So far, you have only one index route: Hello World. Create more routes so that you can retrieve all the movies from the database and save them.

## Set Up the Middleware

Set up the middleware before all the routes, as follows:

```javascript
app.use(require('./middleware/db').connectDisconnect);
```

## Set up Secrets, Bundling, and Watching

Recall that you ran the `create` command on the `index.js` file earlier. Now perform these tasks:

1. Bundle two files \(`index` and `db`\).
2. Specify the secrets \(`env`\).
3. Watch for the changes.

Run this command line for them all:

```bash
wt create index --secret MONGO_URL=<MONGOLAB-CONNECTION-URL> --bundle --watch
```

Grab the URL from the database home:

![](https://d2mxuefqeaa7sj.cloudfront.net/s_C4E0BB4A3CA481FA22D9AA6239D953F2B1D94D00408DB28F7AB567E3C6C4DB1A_1521568483599_Screen+Shot+2018-03-20+at+6.53.29+PM.png)

## Create A Movie Route

Add the following route immediately after the `/` route:

```javascript
app.post('/movies', (req, res) => {
  const newMovie = new req.movieModal(
    Object.assign(req.body, { created_at: Date.now() })
  );
  newMovie.save((err, savedMovie) => {
    res.json(savedMovie);
  });
});
```

The above code does the following:

1. Create a new movie with the movie modal.
2. Save that movie and return the saved version to the client.

Finally, test with [Postman](https://www.getpostman.com):

![](https://d2mxuefqeaa7sj.cloudfront.net/s_C4E0BB4A3CA481FA22D9AA6239D953F2B1D94D00408DB28F7AB567E3C6C4DB1A_1521567767849_Screen+Shot+2018-03-20+at+6.42.29+PM.png)

## Retrieve All the Movies

Add this route to retrieve all the existing movies:

```javascript
app.get('/movies', (req, res) => {
  req.movieModal
    .find({})
    .sort({ created_at: -1 })
    .exec((err, movies) => res.json(movies));
});
```

Subsequently, that route finds the movies, sorts them by date in reverse order, and returns them:

![](https://d2mxuefqeaa7sj.cloudfront.net/s_C4E0BB4A3CA481FA22D9AA6239D953F2B1D94D00408DB28F7AB567E3C6C4DB1A_1521568015585_Screen+Shot+2018-03-20+at+6.46.48+PM.png)

