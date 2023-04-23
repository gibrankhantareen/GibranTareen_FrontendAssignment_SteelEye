# My Answers

Part by Part Explanations:



## Answer 1:
The simple List component in ReactJS is a component that takes an array of items as a prop and renders a list of Single List Item components for each item in the array. The Single List Item component is a simple component that displays the text of the item and changes its background color when it is selected. The List component is responsible for maintaining the selected state and passing it down to the Single List Item components. The List component also makes use of React hooks, specifically useState and useEffect hooks, to manage the selected state and to reset it when the items prop changes. It also uses the React.memo higher-order component to optimize rendering and reduce unnecessary re-renders.

Overall, the simple List component is a basic example of how to create a list in ReactJS and is a building block for more complex list components that can be used in larger applications.



## Answer 2:
There are a few problems/warnings with the code:

    1. The setSelectedIndex hook in the WrappedListComponent component is not correctly defined. The first value in the useState hook should be the initial value, but it is instead the function to update the value. The correct definition should be: const [selectedIndex, setSelectedIndex] = useState(null);

    2. The propTypes definition for the items prop in the WrappedListComponent component is not correct. The PropTypes.array() method should have PropTypes.shape({...}) as an argument instead of PropTypes.shapeOf({...}).

    3. The onClickHandler function in the WrappedSingleListItem component is not correctly defined. It should be wrapped in an anonymous function to prevent it from being called on render. The correct definition should be: onClick={() => onClickHandler(index)}.

## Answer 3

```bash
import React, { useState, useEffect, useCallback, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const SingleListItem = memo(({ index, isSelected, onClickHandler, text }) => {
  const handleClick = useCallback(() => {
    onClickHandler(index);
  }, [onClickHandler, index]);

  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red' }}
      onClick={handleClick}
    >
      {text}
    </li>
  );
});

SingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

// List Component
const List = memo(({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = useCallback(index => {
    setSelectedIndex(index);
  }, []);

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={handleClick}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
});

List.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

List.defaultProps = {
  items: [],
};

export default List;

```
Explanation:

    1. The SingleListItem component now uses useCallback to memoize the handleClick function and prevent unnecessary re-renders of the component. The onClickHandler function is also now correctly wrapped in an anonymous function to prevent it from being called on render.
    2. The PropTypes definition for the items prop in the List component is fixed to use PropTypes.arrayOf instead of PropTypes.array(). The defaultProps value is also changed to an empty array instead of null.
    3. The List component now includes a key prop on each SingleListItem component to improve rendering performance.
    4. The setSelectedIndex hook in the List component is now correctly defined with an initial value of null.
    5. The handleClick function in the List component is now memoized with useCallback to prevent unnecessary re-renders of the component.


## Authors

- [@gibrankhantareen](https://www.github.com/gibrankhantareen)


## Feedback

If you have any feedback, please reach out to me at gibrankhantareen@gmail.com
