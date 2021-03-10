Checkout `gh-pages`.
```
git checkout master
git checkout gh-pages
git checkout -b feature/update-gh-pages
git merge master
```

Install sphinx and the related libraries.
```
pip install sphinx sphinx_rtd_theme recommonmark
```

Generate docs.
```
sphinx-apidoc -f -o ./docs/source ./handyrl
sphinx-apidoc -f -o ./docs/source ./handyrl/envs
sphinx-apidoc -f -o ./docs/source ./handyrl/envs/kaggle
```

Build docs.
```
cp -r docs/tutorial docs/documentation docs/source
sphinx-build -b html ./docs/source ./docs
```
