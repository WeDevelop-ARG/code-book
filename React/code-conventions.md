# Code Conventions for React

  These include the [**Airbnb style guides**](https://github.com/airbnb/javascript/tree/master/react) with a couple of changes.

## Classes vs Functional Components

- Prefer Functional Components with Hooks over Class based components

  ```jsx
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
  ```

## Naming

   - **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.jsx`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // not good
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // not good
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.js` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name:

    ```jsx
    // not good
    import Footer from './Footer/Footer';

    // not good
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```

  - **Higher-order Component Naming**: Use a composite of the higher-order component’s name and the passed-in component’s name as the `displayName` on the generated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.

    > Why? A component’s `displayName` may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.

    ```jsx
    // not good
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // good
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

  - **Props Naming**: Avoid using DOM component prop names for different purposes.s

    > Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

    ```jsx
    // not good
    <MyComponent style="fancy" />

    // not good
    <MyComponent className="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

## Alignment

  - Multi-line props should be indented a level deeper

    ```jsx
    // not good
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />
    ```

  - Single prop should be a one-liner

    ```jsx
    <Foo bar="bar" /> 
    ```

  - Children should be indented one level deeper

  ```jsx
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
  ```
  - Conditional rendering should:

    - Treated as a () block:

     ```jsx
    {showButton && (
      <Button />
    )}
    ```

    - Be a one-liner

    ```jsx
    {showButton && <Button />}
    ```

## Spacing

  - Include a single space before closing tag

    ```jsx
    <Foo /> 
    ```
  - Do not pad JSX curly braces with spaces

    ```jsx
    // not good
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
    ```

## Props

  - Use **camelCase** for prop names
  - Omit the value of the prop when it is explicitly `true`

    ```jsx
    // not good
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />

    // good
    <Foo hidden />
    ```

  - Always define explicit defaultProps for all non-required props.

  > Why? propTypes are a form of documentation, and providing defaultProps means the reader of your code doesn’t have to assume as much. In addition, it can mean that your code can omit certain type checks.

  ```jsx
  // not good
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

  // good
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
  ```

## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line.

    ```jsx
    // not good
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;

    // good
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );

    // good, when single line
      return <MyComponent>{body}</MyComponent>;
    ```

## Tags

  - Always self-close tags that have no children.

    ```jsx
    // not good
    <Foo variant="stuff"></Foo>

    // good
    <Foo variant="stuff" />
    ```

  - If your component has multiline properties, close its tag on a new line.

    ```jsx
    // not good
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```
