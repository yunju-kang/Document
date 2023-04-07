# YAML Grammar

### Tags

- Setting a custom URL => `%TAG ! tag:hostdata:phl:`
- Setting local tags => `!`
- Setting a data type =? `!!str`

### Anchors

- save variable value => `&a apple`
- get saved variable value => `*a`

### Bracket

- list -> `[a, b, c]`

  ```yaml
  language:
  	- Korean
  	- English
  	- Chinese
  ```

- one line -> `{a:apple, b:banana, c:cherry}

  ```yaml
  fruit :
  	a : apple
  	b : banana
  	c : cherry
  ```

  ​

