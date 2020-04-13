# Vue

## Getting Started

### Préparation

Afin de boostraper un projet Vue, il est nécessaire d'utiliser la package `@vue/cli` :

  `yarn global add @vue/cli`
  `npm install -g @vue/cli`

### Génération du project

Afin de générer l'application, utiliser la commande suivante : `vue create nomproject`

## Notions de base

### Création d'un composant

Un composant est composé de 3 parties :

- Un template (ce qui va être affiché dans le nom)
- Un object javascript
- Un style

```xml
<template>
  <button>Test</button>
</template>
```

```javascript
export default {
  name: 'Button'
};
```

```css
<style>
button {
  background-color: red;
}
</style>
```

L'objet doit comporter un attribut `name` afin d'identifier le composant.

Il existe deux types de styles en Vue, style global ou scopé. Le style global sera appliqué pour tout le DOM alors que le scopé ne sera appliqué que pour le composant courant.

- Global: `<style>`
- Scopé: `<style scoped>`

Afin d'utiliser un autre composant, on doit l'importer puis l'injecter dans une propriété `components`. Ces composants injectés pourront alors être utiliser en tant que balise xml.

```xml
<template>
  <Children/>
</template>
```

```javascript
import Children from "./Children";

export default {
  name: 'Parent',
  components: {
    Children
  }
};
```

### Passage de données d'un composant parent à un composant enfant

#### Les props

Afin de passer des données d'un composant parent à un composant enfant, on utilise des **props** ou attributs. Ces attributs sont directement envoyés par le template vers les arguments du composant enfant. On peut envoyer des chaines de caractères (entre quotes) ou bien des variables javascript en utilisant `v-bind`. Ces **props** sont en read-only, il n'est en aucun cas possible de modifier la valeur d'une variable d'un composant parent qui a été passé par les **props**.

```xml
<!-- Template Composant parent: passage d'un titre à un composant Button -->
<template>
  <Button title="Test" v-bind:count="count" :color="color"/>
</template>
```

```javascript
// Composant enfant: Button
export default {
  name: "Button",
  props: {
    title: String,
    count: Number,
    color: String
  }
};
```

Dans l'exemple précédent, le composant parent définit différents attributs qui seront envoyés au composant Button.

- title: attribut définit et envoyé directement par le template
- count: attribut définit dans le composant parent et envoyé par le template `v-bind`
- count: attribut définit dans le composant parent et envoyé par le template `:` (raccourci de v-bind)

#### Event Listener

Il existe des **props** spéciaux qui permettent d'écouter un événement sur certains éléments du DOM comme un `button` ou un `input`. Ces attributs sont préfixés par **v-on:** ou **@** suivi du nom de l'événement en snakecase, ex: `v-on:click`, `@click`. On peut donc leur attribuer une fonction contenue dans une méthode.

```xml
<template>
  <button @on:click="handleClick">{{ title }}</button>
</template>
```

```javascript
export default {
  name: "Button",
  props: {
    title: String
  },
  methods: {
    handleClick: () => console.log('clicked')
  }
};
```

### Les structures conditionnelles

Afin d'effectuer des conditions sur ce que l'on veut "afficher" dans le DOM, on peut utiliser l'ensemble d'attributs spéciaux `v-if`, `v-else`, `v-else-if` auquels on associe une expression javascript.

```xml
<template>
  <div>
    <div v-if="page === 'login'">
      <Login/>
    </div>
    <div v-else-if="page === 'forgot-password'">
      <ForgotPassword/>
    </div>
    <div v-else>
      <Register/>
    </div>
  </div>
</template>
```

Nous pouvons aussi utiliser l'attribut spécial `v-show`. La différence entre `v-if` et `v-show` réside dans le fait que `v-if` n'ajoute pas l'élément dans le DOM alors que `v-show` va l'ajouter avec la propriété css **display: none;**

### Génération de list

Afin de générer une liste d'élément venant d'un tableau, on peut utiliser l'attribut `v-for` auquel on associe une expression javascript.

```xml
<template>
  <ul>
    <li v-for="(item, index) in items" :key="item.id">{{ index }} - {{ item.name }}</li>
  </ul>
</template>
```

La propriété `key` permet à VueJS d'optimiser les rechargements d'une liste de données. Elle lui permet de reconnaître quel composant a besoin d'être recharger en conséquence de n'importe quelle opération (ajout, suppression, modification). Cette clé doit donc être unique par élément au sein d'une même liste, il convient d'utiliser un identifiant unique basé sur la data à afficher que d'utiliser son index. En effet, à chaque suppresion/ajout les tableaux sont ré-indexés, de ce fait, React va alors recharger un grand nombre de composant.

### Rendre les composants dynamiques

Afin de rendre les composants plus dynamiques, on va introduire la notion de **state** ou état d'un composant.

Afin de mettre en place cet état, on va utiliser la proriété `data`. Celui-ci prend en argument une fonction générant un objet.

```xml
<template>
  <button @click="handleClick">{{ title }} - Clicked {{ count }} times</button>
</template>
```

```javascript
export default {
  name: "Button",
  props: {
    title: String
  },
  data: function () {
    return {
      count: 0
    };
  }
  methods: {
    handleClick: () => this.count += 1
  }
};
```

Dans l'exemple précédent, à chaque clique sur le bouton, le composant va modifier son état en ajoutant 1 à la valeur de count. Celui-ci va donc se recharger en affichant la nouvelle valeur de count.

Si un composant enfant veut modifier l'état d'un composant parent, ce dernier doit passer une fonction par les **props** au composant enfant.

Composant Parent

```xml
<template>
  <div>
    {{ count }}
    <Button :handleClick="handleClick">{{ title }}</Button>
  </div>
</template>
```

```javascript
import Button from "./Button";
export default {
  name: "Body",
  components: {
    Button
  },
  data: function () {
    return {
      count: 0
    };
  }
  methods: {
    handleClick: () => this.count += 1
  }
};
```

Composant Button

```xml
<template>
  <button @click="handleClick">{{ title }} - Clicked {{ count }} times</button>
</template>
```

```javascript
export default {
  name: "Button",
  props: {
    handleClick: Function
  }
};
```

Dans l'exemple précédent au clique sur le `button`, l'event listener `onClick` est déclenché dans le composant `Button` appelant ainsi la méthode `handleClick` provenant du composant `Body` ajoutant +1 au compteur.

### Cycle de vie d'un composant

Il existe plusieurs méthodes qui permettent de gérer le cycle de vie d'un composant Vue.

- created
- mounted
- updated
- destroyed
- ...

```javascript
export default {
  name: "Button",
  mounted: function () {
    console.log("mounted");
  }
};
```

### Gestion des formulaires

Vue simplifie le traitement des inputs d'un formulaire en proposant un attribut spécial appelé `v-model`. Celui-ci se charge de la mise à jour de la valeur du composant à partir d'une **data**.

```xml
<template>
  <form>
    <input v-model="username"/>
    <input v-model="password"/>
  </form>
</template>
```

```javascript
<script>
  export default {
    name: 'AuthForm',
    data: function () {
      return {
        username: '',
        password: ''
      };
    }
  }
</script>
```

## Notions avancées

### L'injection de dépendance

Avec l'injection de dépendances, on va pouvoir diffuser des méthodes et des propriétés à n'importe quel composant Vue situé sous le composant en question.
Pour utiliser l'injection de dépendance, on va utiliser deux nouvelles propriétés `provide` et `inject`.

- **provide**: Permet de diffuser des valeurs et des méthodes dans l'arborescence de composant
- **inject**: Permet de récupérer des valeurs et des méthodes qui ont été diffusées

Provider

```javascript
export default {
  name: "AuthProvider",
  data: function () {
    return {
      user: null
    };
  },
  methods: {
    login: function (user) {
      this.user = user;
    },
    logout: function () {
      this.user = null;
    }
  },
  provide: function () {
    return {
      user: this.user,
      login: this.login,
      logout: this.logout
    }
  }
};
```

Consumer

```xml
<template>
  <div>
    <button v-if="user === null" @click="login">Log In</button>
    <button v-if="user !== null" @click="logout">Logout</button>
  </div>
</template>
```

```javascript
export default {
  name: "AuthButton",
  inject: ['user', 'login', 'logout']
};
```

### Les modifiers

Les **modifiers** sont des sucres syntaxiques apportés par Vue qui permet de faire des traitements automatiques que l'on a l'habitude de faire en javascript. Ils en existent une multitude et ils peuvent être chaînés.

```xml
<template>
  <form @submit.prevent="handleSubmit">
    <input v-model="username"/>
  </form>
</template>
```

Dans l'exemple précédent, Vue va d'abord effectuer un `event.preventDefault()` avant de lancer la fonction handleSubmit.

```xml
<template>
    <input v-model.number="phonenumber"/>
</template>
```

Dans l'exemple précédent, Vue va automatiquement convertir la valeur de phonenumber en type Number

```xml
<template>
    <input @keyup.v="onVKeyPressed"/>
</template>
```

Dans l'exemple précédent, Vue va écouter uniquement lorsque l'utilisateur aura appuyer sur la touche **V**.
