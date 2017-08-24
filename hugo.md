# Hugo

Create a new post

```
hugo new post/good-to-great.md
```

Start the hugo server and include the drafts:

```
hugo server --buildDrafts
```

In order to use a specific theme add the following to every command:

```
--theme=blackburn
```

Publish a draft:

```
hugo undraft content/post/good-to-great.md
```

Generate the site:

```
hugo --theme=blackburn
```

Copy the stuuf to the actual blog:
```
copy /Y public ..\amecky.github.io\
```