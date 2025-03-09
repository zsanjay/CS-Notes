
## `React.memo()` and `useMemo()`

From the example above, we can see the major differences between `React.memo()` and `useMemo()`:

- `React.memo()` is a higher-order component that we can use to wrap components that we do not want to re-render unless props within them change
- `useMemo()` is a React Hook that we can use to wrap functions within a component. We can use this to ensure that the values within that function are re-computed only when one of its dependencies change

