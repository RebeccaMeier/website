# Regex

## To get info about image from html and put it in the yaml format.

```regex
^.*title="([^"]*)".*description="([^"]*)".*uploads\/([^"]*)\.jpg".*$
- thumbnail: "/img/IV/$3.jpg"\n  src: "/img/IV/$3a.jpg"\n  title: "$1"\n  description: "$2"\n  size: "2917x1524"
```
