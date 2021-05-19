# Hoops Web Viewer Vue.js Demo
This is a demo for intergrating [Hoops Web Platform](https://www.techsoft3d.com/products/hoops/communicator/) (HWP) with [Vue.js](https://v3.vuejs.org/) framework. It is bootstraped with [Vue CLI](https://cli.vuejs.org/) and utilizes typescript.

## Live Demo
A live demo is running with GitHub Pages at: https://techsoft3d.github.io/hwp-demo-vue/ 

## For Local Using

### `npm install`
Install node_module dependencies.

### `npm run serve`
Compiles and hot-reloads for development.

### `npm run build`
Compiles and minifies for production.

## Steps to Integrate HWP with Vue.js
Please make sure that NodeJS and Vue CLI are both installed.
1. Create a new project with `vue create <new-project>`. Make sure to add TypeScript by selecting it with the space key,
2. Copy the type definition files `hoops_web_viewer.d.ts` and `tcc.d.ts` into to the `src/@types` folder. They can be found under `/web_viewer/typescript/` folder of your local installation.
3. Include the `hoops_web_viewer.js` with a script tag in the header of index.html. It can be located under the` /web_viewer/src/js/ folder` of your local installation.
