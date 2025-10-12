# Grex Documentation

Grex documentation to run locally.

## Contribute to the documentation

If you want to contribute to the documentation, you first need to clone the repository and retrieve the theme
(which is a submodule). To this end, execute the following commands:

```console
git clone git@github.com:Autogramm/grex-doc.git
cd grex-doc
git submodule init
git submodule update
```
Install Hugo

- in macOS:
```Linux
apt install hugo 
```
- in macOS:
```console
brew install hugo
```


Then you can run the document using the following command:

```console
hugo server -D
```

The website can then be accessed via the following URL `http://localhost:1313`.

## Troubleshooting

If you are on Ubuntu and the config file is not found when you run `hugo server -D`, try using the snap package:

```console
snap install hugo
snap run hugo server -D
```
