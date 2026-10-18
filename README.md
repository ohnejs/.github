# ohnejs/.github

The files every repository of the ohne family shares.

- `.github/ISSUE_TEMPLATE/`: GitHub shows these forms in every `ohnejs` repository that has none of its own.
- `SECURITY.md` and `CODE_OF_CONDUCT.md`: linked from every repository's sidebar.
- `profile/README.md`: the organization's front page.
- `labels.json`: the labels every repository has.

The issue forms set labels from `labels.json`. A label missing from a repository is skipped silently, so every repository carries the whole set. To add or update them everywhere:

```sh
for repo in $(gh repo list ohnejs --limit 100 --json name --jq '.[].name'); do
  node -e 'for (const l of require("./labels.json")) console.log([l.name, l.color, l.description].join("\t"))' |
    while IFS="$(printf '\t')" read -r name color description; do
      gh label create "$name" --repo "ohnejs/$repo" --force --color "$color" --description "$description"
    done
done
```

It adds and updates labels and never deletes one, so a label added by hand on GitHub survives.

The issue types the forms use (Bug, Feature, Task) live in the organization settings.
