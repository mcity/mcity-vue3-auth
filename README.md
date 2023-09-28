# Mcity Vue3 Auth

Vue wrapper files used to check authorization for users throughout Mcity apps.

## Tech

Mcity-vue3-auth uses a number of open source projects to work properly:

- [[Vue.js](https://vuejs.org/) v3] - Used for our frontend.
- [[Pinia](https://pinia.vuejs.org/)] - Used for frontend state management.

## Env Setup

- Create file `.env.local` if it does not already exist.
- Add the following variables to this file.

```
VITE_MIXPANEL_TOKEN=value
VITE_OAUTH_KEY=value
VITE_HOST=http://localhost:8080
VITE_OAUTH_SERVER=https://keys.um.city/
VITE_REDIRECT_URI=http://localhost:8080/authorized
VITE_OAUTH_SCOPE=email+roles+[value]
```

## App Setup

- After the default router view in the `App.vue` template, add a named router view to `App.vue` with `<router-view name="auth"/>`
- Import `authRefresh` component with
  `import authRefresh from 'mcity-vue3-auth/components/RefreshAuthiFrame.vue'`
- In the template of `App.vue`, after the router-view, add the imported `authRefresh` component

```
<auth-refresh
  v-if="getShowIframe"
  adminRole="PROJECTADMIN"
>
</auth-refresh>
```

## Router Setup

- Import `AuthComponent` into the Vue Router `index.js` file with `import 'AuthComponent' from 'mcity-vue3-auth/components/AuthComponent.vue'`
- Add a new top-level route, `/authorized`

```
{
  path: '/authorized',
  name: 'OverviewAuth',
  components: {
    auth: AuthComponent
  },
  meta: {
    authorized: true
  }
},
```

- Import the `checkRequiresAuth` function into the Vue router `index.js` file with
  `import { checkRequiresAuth } from 'mcity-vue-auth/router/beforeEachHooks'`
- If it is not already there, import the vuex store into the router.
  `import store from '../store'`
- Add the following line before the export of the router:
  `router.beforeEach(checkRequiresAuth)`

## Pinia Setup

- Add the `session` Pinia module to the store.

```
import useSessionStore from '@mcity/mcity-vue-auth-pinia/dist/store/session'

export const useStore = defineStore('store', () => {

  const session = useSessionStore()

  return { projects, getProjectByKey, session }
})
```

# Adding Changes

## Babel Transpilation

- After making code changes, rerun babel by running `npm run build`
- Make sure to commit these built files.
