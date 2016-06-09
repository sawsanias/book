# Step 3

You have probably noticed that our component has a bug: it says "Good morning" at any time of the day. This is a good example of a small amount of logic you often need in your front-end components.

To maintain things tidy and easy to test, [revenge](https://github.com/buildo/revenge) provides another decorator, `@skinnable`. Let's import it and add it to our component:

```js
import { pure, skinnable } from 'revenge';

...
@connect({ formal: t.maybe(t.Boolean) })
@pure
@skinnable()
@props({
  transition: t.Function,
  formal: t.maybe(t.Boolean)
})
export default class Hello extends React.Component {
```

The new decorator requires your component to implement two methods:

1. `template()`, which will replace your `render()` method, and receive an object containing the properties it needs

```js
template({ toggle, greeting }) {
  return (
    <div className='hello'>
      <h1>
        <a onClick={toggle}>{greeting}</a>
      </h1>
    </div>
  );
}
```

2. `getLocals()`, which will contain the logic and return the object expected by the `template()` method above

```js
getLocals() {
  const { toggle } = this.props;
  const greeting = this.props.formal ? this.formalGreeting() : 'Hello';

  return { toggle, greeting };
}
```

Finally, let's implement a naive way to generate the correct greeting:

```js
formalGreeting() {
  const hours = new Date().getHours();
  if (hours > 20) return 'Good Night';
  else if (hours > 13) return 'Good Afternoon';
  else return 'Good Morning';
}
```

## Step2->Step3
Check the [full diff](https://github.com/buildo/webseed/compare/tutorial-step2...tutorial-step3) between Step 2 and Step 3.