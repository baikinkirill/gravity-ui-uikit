# Toaster

Component for adjustable notifications.

## Usage with context

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import {ToasterComponent, ToasterProvider} from '@gravity-ui/uikit';

ReactDOM.render(
  <ToasterProvider>
    <App />
    <ToasterComponent className="optional additional classes" />
  </ToasterProvider>,
  document.getElementById('root'),
);
```

Then in your app components you can show toasts with `useToaster` hook.

```jsx
import {useToaster} from '@gravity-ui/uikit';
import {useEffect} from 'react';

export function FoobarComponent() {
  const {add} = useToaster();

  useEffect(() => {
    add({
      title: 'Toaster is here',
    });
  }, []);

  return null;
}
```

Hook returns methods `add`, `update`, `remove` and `removeAll` (see below).

## Usage as HOC

For class components you can use `withToaster` HOC. This will inject `toaster`
prop to component.

```jsx
import {Component} from 'react';
import {withToaster} from '@gravity-ui/uikit';

class FoobarComponent extends Component {
  render() {
    this.props.toaster.add({});
  }
}

const FoobarWithToaster = withToaster()(FoobarComponent);
```

## Usage as singleton

```js
import {Toaster} from '@gravity-ui/uikit';
const toaster = new Toaster();
```

```js
import {toaster} from '@gravity-ui/uikit/toaster-singleton';
```

Toaster has singleton, so when initialized in different parts of the application, the same instance will be returned.
On initialization it is possible to pass className, that will be assigned to dom-element which is wrapping all toasts.

## Constructor arguments

| Parameter | Type      | Default     | Description                                         |
| :-------- | :-------- | :---------- | :-------------------------------------------------- |
| className | `string`  | `undefined` | Custom class name to add to the component container |
| mobile    | `boolean` | `false`     | Configuration that manages mobile/desktop views     |

## Methods

| Method name                   | Params             | Description                                                                                                                             |
| :---------------------------- | :----------------- | :-------------------------------------------------------------------------------------------------------------------------------------- |
| add(toastOptions)             | `Object`           | Create new notification                                                                                                                 |
| remove(name)                  | `string`           | Delete existing notification manually                                                                                                   |
| update(name, overrideOptions) | `string`, `Object` | Change already rendered notification content. In `overrideOptions` following fields are optional: `title`, `type`, `content`, `actions` |

## More about `add`

Accepts argument `toastOptions` with ongoing notification details:

| Parameter       | Type            | Required | Default     | Description                                                                                                                                                         |
| :-------------- | :-------------- | :------- | :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name            | `string`        | yes      |             | Notification unique name. Notifications with same names collapse into one                                                                                           |
| title           | `string`        | yes      |             | Notification title                                                                                                                                                  |
| className       | `string`        |          |             | CSS-class                                                                                                                                                           |
| allowAutoHiding | `boolean`       |          | `true`      | Configuration that manages notification automatic hiding                                                                                                            |
| timeout         | `number`        |          | 5000        | Time (in milliseconds)after which the notification will hide                                                                                                        |
| content         | `node`          |          | `undefined` | Notification content. [Anything that can be rendered: numbers, strings, elements or an array](https://reactjs.org/docs/typechecking-with-proptypes.html#proptypes)  |
| type            | `string`        |          | `undefined` | Notification type. Possible values: `error`, `success`. If `type` is set, icon (success/error) will be added into notification title. _By default there is no icon_ |
| isClosable      | `boolean`       |          | `true`      | Configuration that manages visibility of cross icon, which allows to close notification                                                                             |
| actions         | `ToastAction[]` |          | `undefined` | Array of [actions](./types.ts#L9) which displays after `content`                                                                                                    |

Every `action` is an object with following parameters:

| Parameter        | Type                                      | Required | Default    | Description                                                 |
| :--------------- | :---------------------------------------- | :------- | :--------- | :---------------------------------------------------------- |
| label            | `string`                                  | yes      |            | Action text description                                     |
| onClick          | `() => void`                              | yes      |            | On action click handler                                     |
| view             | [`ButtonView`](../Button/README.md#props) |          | `outlined` | Appearance of the action, same to `view` of the `<Button/>` |
| removeAfterClick | `boolean`                                 |          | `true`     | If notification should be closed after click on action      |
