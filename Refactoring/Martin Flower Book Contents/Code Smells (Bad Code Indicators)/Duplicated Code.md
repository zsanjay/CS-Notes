
Duplicated Code does not need further explanation. According to Fowler, redundant code is one of the worst smells [[1](https://luzkan.github.io/smells/duplicated-code#sources)]. One thing is that this makes it more challenging to read the program. Checking whether the copies are identical is yet another issue - looking at whether there are for sure no tiny differences between code blocks in search of [Oddball Solution](https://luzkan.github.io/smells/oddball-solution) further unnecessarily absorbs developer time. Yet another thing is that whenever a change is made, one needs to check if this should have happened to just one or all of the existing copies of code, wherever they are.

### Refactoring:

- Extract Class
- Extract Function
- Pull Up Method
- Slide Statement
- Form Template Method

