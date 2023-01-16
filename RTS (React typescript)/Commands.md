# Commands

### Create React app with typescript
``` bash
npx create-react-app rts --template typescript 
```

### Find and copy code
```bash
ag . | sk --delimiter ':' --preview 'bat --color=always --highlight-line {2} {1}' |awk -F ":" '{print $3}' |sed 's/^\s+//' | pbcopy
```


### Find and open code
```bash
vim $(ag . | sk --delimiter ':' --preview 'bat --color=always --highlight-line {2} {1}' |awk -F ":" '{print "+"$2" "$1}')
```