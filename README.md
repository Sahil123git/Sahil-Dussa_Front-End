###Front-end Assignment

###Ques 1)=> Explain what the simple List component does.

Answer 1: The List component in React is responsible for displaying a list of items using the SingleListItem component. When the List component is rendered, it generates a SingleListItem component for each item in an array, passing in the text and isSelected props to display the item's content and indicate whether it is selected or not.

The List component also manages the selectedIndex state variable to keep track of the currently selected item. When a user clicks on a SingleListItem component, the handleClick function updates the selectedIndex state variable accordingly.

To apply some basic styling to the list, the List component renders a ListContainer component that wraps all of the SingleListItem components. Overall, this component serves as an example of how to display and interact with a list of items in React.

###Ques 2)=> What problems / warnings are there with code?

Answer 2: 
a. There is a syntax error in the useState hook.

const[selectedIndex,setSelectedIndex]=useState();

b. The onClickHandler can be called as an arrow function.

onClick={()=>onClickHandler(index)}

c. The isSelected prop should be equal to the index.

    <SingleListItem
      key={index}
      onClickHandler={() => handleClick(index)}
      text={item.text}
      isSelected={selectedIndex === index}
    />

d.  The array prop should be replaced with arrayOf, and the shapeOf prop should be replaced with the shape property.

WrappedListComponent.propTypes = { items: PropTypes.array(PropTypes.shapeOf({ text: PropTypes.string.isRequired, })), };

e.  The handleClick function should be corrected.

const handleClick = (index) => {
selectedIndex === index ? setSelectedIndex(null) : setSelectedIndex(index);
};

f. It is recommended to provide a unique key to props when using the map function on an array.

    <SingleListItem
      key={index}
      onClickHandler={() => handleClick(index)}
      text={item.text}
      isSelected={selectedIndex === index}
    />



g. We should avoid using null as a default prop value; a valid value should be assigned.

items: [
    { text: "Name: Sahil Dussa" },
    { text: "Reg No. : 12345678" },
    { text: "Email-id: email@gmail.com" },
    { text: "College: LPU" },
  ]

###Q3) Please fix, optimize, and/or modify the component as much as you think is necessary.
```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  //const [setSelectedIndex, selectedIndex] = useState(); ERROR
  const [selectedIndex, setSelectedIndex] = useState(); //FIXED

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    selectedIndex === index ? setSelectedIndex(null) : setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: "left" }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ),
};

WrappedListComponent.defaultProps = {
  items: [
    { text: "Name: Sahil Dussa" },
    { text: "Reg No. : 12345678" },
    { text: "Email-id: email@gmail.com" },
    { text: "College: LPU" },
  ],
};

const List = memo(WrappedListComponent);

export default List;
```