# luggage
[![build status](https://img.shields.io/travis/luggagejs/luggage/master.svg?style=flat-square)](https://travis-ci.org/luggagejs/luggage)

Dropbox file API wrapper for storing json data.

> Have you ever noticed that their stuff is shit and your shit is stuff?
(George Carlin)

### Usage

```js
const backend = new DropboxBackend(token);
const store = new Luggage(backend);
const articles = store.collection('articles');
```

### Filtering

```js
articles.where({ author: 'John Doe' }).read().then((articles) => {
  console.log('John\'s articles:', articles);
});

/* or you can provide a function */
articles.where(article => article.authors.includes('John Doe')).read().then((articles) => {
  console.log('John\'s articles:', articles);
});

/* Listen to filtered data updates */
articles.where({ author: 'John Doe' }).on('data', (articles) => {
  console.log('John\'s articles:', articles);
});

/* You can stack conditions */
articles.where({ author: 'John Doe' }).where(article => article.comments > 0)

/* or more readable */
articles.where({ author: 'John Doe' }).and(article => article.comments > 0)

```

### Finding single record

```js
/* Collection#find returns the first record found */
articles.find({ author: 'John Doe' }).read().then((article) => {
  console.log('John\'s article:', article);
});

/* Collection#find takes a function */
articles.find(article => article.author === 'John Doe').read().then((article) => {
  console.log('John\'s article:', article);
});

/* Listen to single record updates */
articles.find({ author: 'John Doe' }).on('data', (article) => {
  console.log('John\'s article:', article);
});
```

### Updating record

```js
/* Simple merge with existing record */
articles.find({ id: 1 }).update({ author: 'Jane Doe' }).then(([article]) => {
  console.log('Author changed:', article.author);
});

/* Record#update takes a function (surprise :)) */
articles.find(article => article.id === 42).update((article) => {
  article.authors.push('Jane Doe');
  return article;           // Do not forget to return new record
})
```

### Adding new record

```js
articles.add({ author: 'John Doe', body: 'Blah blah blah mr. Freeman' }).then(([article]) => {
  console.log('New article was added:', article);
});
```

### Deleting record

```js
articles.find({ id: 1 }).delete().then(([article]) => {
  console.log('No longer within collection:', article);
});
```
