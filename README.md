# Vue3

# Vue CLI

## Setup
1. Install Node - check node -v
2. Install Vue CLI globally
	- npm install -g @vue/cli

## Setup Vue Project
1. Install Vue
	- vue create vue_project		// this "vue" we get via Vue CLI
	- Open the folder code .

## Basics
- We will import the things we need from "Vue" library then use it like;
i. import { createApp } from 'vue'		// grabs 'createApp' function from 'vue' lib in node_modules
ii. import App from './App.vue' 	// import main "Vue" component and mount it to root id "#app" - Root Component for SPAs
iii. main.js is the starter point for Vue app

### Vue Component
- Every Vue component contains; 1.Template (mand), 2.Script(optional), 3.Style(optional)

### Vue Styles
- <style></style> 	// apply to all components, as injected in header
- <style scoped></style> 	// apply to only selected component, also injected in the header: Scoped Global CSS

### Run Vue Project
- To Install dependencies: npm install
- To run: npm run serve

### Template Refs
- ref="testRef"		// user this ref to manipulate elements
- in js this.$refs.testRef

## Vue Props
- To avoid data inconsistency in child compnents. Always use/declare parent component's data 
- Passing data from parent components to child or nested components.
- Send prop => <Modal username="malikateeq" />
- Accept Prop => props: ['username']

## Custom Events from child components. i.e; close Modal

```js
	// Child Component Event Emit
	closeModel(){
		this.$emit('closeItNow')

		// Pass data to events
		this.$emit('closeItNow', [12.51])
	}
```

```js
	// Parent Component Event Listening
	<Modal @closeItNow="closeMyModal">

	closeMyModal(){
		// code ..
	}
```

## Event Modifiers
```js
	@click.modifierX="func"

	@click.right="func"	// only call func on mouse right click

	// Modifiers
	self, right, left etc. 
```

## Vue Slots
- Are design to pass Vue Template to components

```js
	// Parent Component
	<Modal>
		<h1> This will be in slot content <h1>
		<p> A long paragraph!! </p>
	</Modal>
```

```js
	// Child Component
	<template>
		// code..
		<slot> </slot>	// The slot will render here.
		// code ..
	</template>
```

### Vue Names Slots 

```js
	// Parent Component
	<Modal>
		<template v-slot:links> 
			<a href="#">Abc</a>
			<a href="#">Xyz</a>
		</template>
		<h1> This will be in slot content <h1>
		<p> A long paragraph!! </p>
	</Modal>
```

```js
	// Child Component
	<template>
		// code..
		<slot> </slot>	// The slot will render here.
		// code ..

		// Use named slots 
		<slot name="links"></slot>
	</template>
```

### Vue Teleport

```js
    <teleport to=".modals">	// .modals class
      <Modal>
        <!-- code -->
      </Modal>
    </teleport>
```

## Vue Lifecycle Hooks

Created => Mounted to DOM => Updated => Destroyed

0. beforeCreate()	// No access to data or anything
1. created()	// After component created, no data only template
2. beforeMount() 	// Just before mounted to DOM. Access data, fetch data.
3. mounted()	// Active on DOM
4. beforeUpdate()	// before data re-rendered to the DOM
5. updated()	// After all updated are reflected to the DOM
6. beforeUnmount() 	// Before component remove from the DOM
7. unmounted() 	// After component remove from the DOM - For cleanups

## Vue Router

- Vue intercepts the request and check URL and inject compnents according to routes. 
- install vue router: select Router when installing vue project

```js

    // 1. Import Component
    import About from '../views/About.vue'

    // 2. Register in Router
    {
        path: '/about',      // Route relative to base URL
        name: 'About',   // Name of the route
        component: About // Component to use for this route
    },

    // 3. Use Router with router-link
    <router-link :to="{ name: 'About' }">About</router-link>
        // OR
    <router-link to="/about">About</router-link>

```

### Router Parameters

```js

    // 1. Register in Router
    {
        path: '/user/:id',
        name: 'ViewUser',
        component: ViewUser
    },

```

### Accessing Router Object

```js
	// Router Object: $route

	// Set Params to a route
    <router-link :to="{ name: 'ViewUser', params: { id: 123 } }">View User</router-link>
	
	// To get parameters
	$route.params.id

		// OR

    // Set props=true
    {
        path: '/user/:id',
        name: 'ViewUser',
		component: ViewUser,
		props: true
	},
	// Get props
	export default {
		props: ['id']
	}

```

### Router Redirects

```js

	// Redirections
    {
		path: '/old/path',
		redirect: '/new/path',
	},

	// Fallback Route 404
    {
		path: '/:catchAll(.*)',
        name: 'NotFound',
		component: NotFound,
	},

	// Redirections
	this.$route.go(-1)	// Go back 1 times
	this.$route.go(2)	// Go forward 2 times
	// Redirect
	this.$route.push({ name: "Home" })	// Redirect to route name "Home"

```

## The Composition API

- Reusable logic, functions ...
- Can write pure JS in setup

```js

export default {
	// This setup function will run first before all other
	setup(){
		// data

		// methods

		// computed

		// lifecycle hooks
	}
}

```

### Composition APIs

#### Refs

- Template Refs: For DOM manipulation
- refs are better than reactives
```js
	// 1. Refs
	<p ref="custom1">Blah blah..</p>

	// 2. Acccess In JS
	import {ref} from "Vue"
	const p = ref("custom1"); // p = DOM element

	// 3. Imp:
	// Refs are reactive and also update in DOM

	return { p }
```
#### Reactives

- For DOM manipulation
```js
	// 1. Refs
	<p > {{ name }} </p>

	// 2. Acccess In JS
	import {reactive} from "Vue"
	
	const name = "malik"
	return { name }

	// 3. Imp:
	// Refs are reactive and also update in DOM
```
#### Computives

```js
	// 1. Refs
	<p > {{ name }} </p>

	// 2. Acccess In JS
	import {computed} from "Vue"
	
	setup(){
		const name = computed(() => {
			return "malik"
		})
	}
	
	return { name }
```
#### Watch & Watch Effects

- Watch for any data, var changes
```js
	// 1. Refs
	<p > {{ name }} </p>

	// 2. Acccess In JS
	import {computed} from "Vue"
	
	setup(){
		const name = computed(() => {
			return "malik"
		})


		// - Does not run initially
		// - Watch only given variables
		watch( name, () => {
			// do something..
		})

		// - Always runs initially
		// - Watches any values / variable / dependency inside it.
		// - Does not require a variable name to watch - watch all
		watchEffect( () => {
			// do something..
		})
	}
	
	return { name }
```

#### Props in Composition Components
```js
export default {
	props: ['posts'],
	setup(props)
	{
		console.log(props)
	}
}
```

#### Lifecycle Hooks inside Composition Component
```js
import { onMounted, onUnMounted, onUpdated } from "Vue"
export default {
	setup()
	{
		onMounted( () => {
		})

		onUnMounted( () => {
		})	

		onUpdated( () => {
		})	

		// Other hooks same as base hooks just add "on" before it.
	},

	// Or general Options API hooks
	mounted( () => {
	})
}
```

#### Fetching async data

```js

const posts = ref([])
const error = ref(null)

const load = async () => 
{
	try {
		let data = await fetch("#")
		if(!data.ok)
			throw Error("No data available.")
	}
	catch(err){
		console.log(err.message)
	}
}

// 
load()

```

#### Composition Functions

- Put below code in resuable files.. like composables/getPosts.js
```js
import {ref} from "Vue"
const getPosts = () => {

	const posts = ref([])
	const error = ref(null)

	const load = async () => 
	{
		try {
			let data = await fetch("#")
			if(!data.ok)
				throw Error("No data available.")
		}
		catch(err){
			console.log(err.message)
		}
	}
	return { posts, error, load }	// load function
}
export default getPosts
```

- Using above composables
```js

import {getPosts} from "composables/getPosts.js"
export default{
	setup(){
		const { posts, error, load } = getPosts()
		// Now posts = error = load === null
		load()
		// Now posts, error, will have some values computed from function load


	}
}


```