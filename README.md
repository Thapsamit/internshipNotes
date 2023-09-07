## React Impotant notes

### Difference between render, rerender, painting or repainting of dom

"Render," "re-render," and "painting" or "repainting" are terms used in the context of web development and React to describe different phases of the process by which a user interface is updated and displayed to the user. Let's clarify these terms and their differences:

- Render:

Rendering refers to the process of generating a new virtual representation of a component's user interface based on its current state and props. In React, every time a component's state or props change, the render method (for class components) or the function component itself (for functional components) is re-executed to create a new virtual DOM representation.
During rendering, React calculates what the updated user interface should look like, but it does not directly update the actual DOM on the screen.
Re-render:

- Re-rendering occurs when a component's render method or function component is executed again, typically due to changes in its state or props. It's the process of creating a new virtual DOM representation for the component.
Re-rendering is an internal React process, and it doesn't necessarily mean that the component's changes are immediately visible on the screen. React first calculates the differences (virtual DOM diffing) between the new virtual DOM and the previous one before updating the actual DOM.
Painting/Repainting:

- Painting or repainting refers to the process of updating the actual pixels on the screen to reflect the changes made to the DOM. This happens after React has determined what changes are necessary through virtual DOM diffing.
When React identifies the differences in the virtual DOM, it applies these changes to the actual DOM, which can trigger the browser to repaint or redraw affected elements on the screen. This is a lower-level browser operation that involves updating the pixels on the screen to match the new DOM state.
Painting is an important step in rendering a web page, and it can be a performance bottleneck if there are many updates to the DOM.
