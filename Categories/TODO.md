```base
views:
  - type: table
    name: TODO
    filters:
      and:
        - file.name != "TODOテンプレート"
        - or:
            - categories.contains(link("TODO"))
        - finished.isEmpty()
    order:
      - file.name
      - created
      - finished
    sort:
      - property: created
        direction: DESC
      - property: file.tags
        direction: DESC
      - property: finished
        direction: ASC
    columnSize:
      file.name: 339
      note.created: 166
  - type: table
    name: すべて
    filters:
      and:
        - file.name != "TODOテンプレート"
        - or:
            - categories.contains(link("TODO"))
    order:
      - file.name
      - created
      - finished
    sort:
      - property: finished
        direction: DESC
      - property: created
        direction: DESC
      - property: file.tags
        direction: DESC
    columnSize:
      file.name: 339
      note.created: 166

```
