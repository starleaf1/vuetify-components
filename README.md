# vuetify-components
A collection of Vue components I built using Vuetify

## Components

### `ChangeableText`
This component renders a text with a pencil icon next to it. Click on the pencil to change the text, then click on the check mark to accept the change.
#### Props and Events
This component accepts a `value` prop and emits `input` event, so you can use it with `v-model`. Optionally, it also accepts a prop called `handler`. This prop is a function which will be executed when the check mark is clicked. You can use this to directly submit the value to a server, for example. Note that while `handler` is executing, the component would be disabled.
