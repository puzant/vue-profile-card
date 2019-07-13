# vue-profile-card
profile card built using Vuejs, Typescript, Pug, Sass

## This small App built using the following Technologies 
- VueJs (with single class component)
- Typescript
- Pug Temaplet syntax
- SASS

> This repo includes best practices to write modular & scalable code for VueJs components 

> Vuex wasn't used in this project, due to the project size, instead direct `axios.get()` was used directly in a component 

* the old implemention for the app can be found in `old_code` folder

### Few notes about the old implemention
- no dynamic rendering was used (all the cases were handles manually)
- there was heavy code repitition in the component 
- there was no HTML semantics 
- `HTML` classes were not prefixed 
- `CSS` classes names were odd & confusing 
- direct `CSS` rules were used as HTML class name 
  ex: `justify-content: space-around`, was used like `class="space-around`
- some formatting methods were within the component, while they should be located in a plugin


![Card](https://puzant.github.io/portfolio/assets/project-6.png)
