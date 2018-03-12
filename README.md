<div align="center">
  <h1>:construction: vue-classnames :construction:</h1>
</div>

### Installation
```sh
yarn add vue-classnames
# or
npm i vue-classnames
```

## Activation
There are two ways of activating this package: globally and locally.
If you want to use it on a single component, it would be a good idea to go with the local method:
```js
import { mixin as classNamesMixin } from 'vue-classnames';

Vue.component('my-component', {
  mixins: [classNamesMixin],
});

// or SFC
export default {
  mixins: [classNamesMixin],
};
```

To activate it globally, just use this package as Vue plugin, like so:
```js
import VueClassNames from 'vue-classnames';

Vue.use(VueClassNames);
```

## Usage
<table>
  <tr>
    <th align="center"><code>Heading.vue</code></th>
    <th align="center"><code>Content.vue</code></th>
  </tr>
  <tr>
    <td>
      <pre lang="vue">
&lt;template>
  &lt;div :class="$classNames()">
      &lt;h1>&lt;slot />&lt;/h1>
  &lt;/div>
&lt;/template>
<script>
export default {
  name: 'Heading',
  props: {
    padded: {
      type: Boolean,
      default: false,
      className: 'relaxed',
    },
    large: {
      type: Boolean,
      default: false,
      className: true,
    },
    big: {
      type: Boolean,
      default: false,
      className: ['relaxed', 'large'],
    },
  },
};
</script>
<style>
  .relaxed {
    padding: 1rem;
  }<br>
<br>
  .large {
    font-size: 3rem;
  }
</style></pre>
    </td>
    <td valign="top">
      <pre lang="vue">
&lt;template>
  &lt;section>
      &lt;Heading large padded>SFC demo&lt;/Heading>
      &lt;!-- or simply -->
      &lt;Heading big>SFC demo&lt;/Heading>
  &lt;/section>
&lt;/template>
<script>
import Heading from './Heading.vue';<br>
<br>
export default {
  name: 'Content',
  components: { Heading },
};
</script></pre>
    </td>
  </tr>
  <tr>
    <th colspan="2">Output</th>
  </tr>
  <tr>
    <td colspan="2">
      <pre lang="html">
&lt;section>
  &lt;div class="relaxed large">
    &lt;h1>SFC demo&lt;/h1>
  &lt;/div>
&lt;/section></pre>
    </td>
  </tr>
</table>

## API
#### `vm.$classNames(defaultClassName = null)`
Returns an array of the class names, otherwise `defaultClassName`.


#### `className`
* If `true`, then prop name will be also name of the class.
* If value is a string, then it will be name of the class.
* If value is a function then it will be executed with following object as argument `{ name, value, props }`,
where `name` and `value` are current prop name and value, and `props` contains all props passed to the component.
* If value is array, then each element is treated as a single `className`.

##### NOTE:
If `className` is a function or an array,
then both function output and each array element will be processed as a single `className`, which means that all above rules apply to them.

##### EXAMPLE:
```js
export default {
   props: {
     test1: {
       // `$classNames` will return `['test1']` if `otherProp` is truthy.
       className: ({ props }) => !!props.otherProp,
     },
     test2: {
       // `$classNames` will return `['better-class']` if `someOtherProp` is truthy.
       className: ({ props }) => !!props.someOtherProp && 'better-class',
     },
     test3: {
       // `$classNames` will return `['btn', 'btn-blue']` if `blue` is truthy.
       className: ({ props }) => !!props.blue && ['btn', 'btn-blue'],
     },
     test4: {
       // `$classNames` will return `['test4']` if this prop value is `some-value`.
       className: ({ name, value }) => (value === 'some-value' && name),
     },
     test5: {
       // `$classNames` will return `['btn', 'btn-large', 'btn-relaxed']` if `padded` is truthy,
       // otherwise it will return `['btn', 'btn-large']`.
       className: ['btn', 'btn-large', ({ props }) => !!props.padded && 'btn-relaxed'],
     },
  },
};
```
