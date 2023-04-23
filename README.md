#Question)1=> Explain what the simple List component does.

Answer 1: The List component is a React component that displays a list of items, where each item is represented by a SingleListItem component.

When the List component is rendered, it maps over an array of items and creates a SingleListItem component for each item. It passes the text and isSelected props to each SingleListItem component, which are used to display the text of the item and indicate whether or not the item is currently selected.

The List component also maintains a selectedIndex state variable, which keeps track of the currently selected item. When a SingleListItem component is clicked, it calls a handleClick function that updates the selectedIndex state variable accordingly.

Finally, the List component renders a ListContainer component that wraps all of the SingleListItem components, which applies some basic styling to the list.

Overall, the List component is a simple example of how to render a list of items in React, and how to handle user interactions with those items.

Question)2=> What problems / warnings are there with code?

Answer 2: a. useState syntax error

const[selectedIndex,setSelectedIndex]=useState();

b. isSelected should be equal to index

    <SingleListItem
      key={index}
      onClickHandler={() => handleClick(index)}
      text={item.text}
      isSelected={selectedIndex === index}
    />
c. array should be replaced with arrayOf & shapeOf should be replaced with shape property.

WrappedListComponent.propTypes = { items: PropTypes.array(PropTypes.shapeOf({ text: PropTypes.string.isRequired, })), };

d. onclickHandler could be called as a arrow function

onClick={()=>onClickHandler(index)}

e. Unique key to props , It is recommended to provide unique key to props while using map function on array

    <SingleListItem
      key={index}
      onClickHandler={() => handleClick(index)}
      text={item.text}
      isSelected={selectedIndex === index}
    />
f. handleClick function should be right.

const handleClick = (index) => {
selectedIndex === index ? setSelectedIndex(null) : setSelectedIndex(index);
};

g. null as a default props:- We should not use null as default props there must be some valid value allocated to default props

List.defaultProps = {
items: [{ text: "I" }, { text: "am" }, { text: "placed" }, { text: "hello" }], };

h. We should have different different componenets. It increases code reusability , readability .

Q3) Please fix, optimize, and/or modify the component as much as you think is necessary.

Answer) i have made different different components . you can see by going inside src/components List component is the main component . SingleListItem and ListContainer are used inside List component.

Also i have Implemented all the errors in one component by the name of EverythingInone.js component. here is the code of EverythingInone.js component

//in one component only

import React, { useState, useEffect, memo } from "react"; import PropTypes from "prop-types";

// Single List Item const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => { // const handleClick = () => { // onClickHandler(index); // }; return ( <li style={{ backgroundColor: isSelected ? "green" : "red" }} onClick={() => onClickHandler(index)} > {text} {/*

{text}
*/} ); };
WrappedSingleListItem.propTypes = { index: PropTypes.number, isSelected: PropTypes.bool, onClickHandler: PropTypes.func.isRequired, text: PropTypes.string.isRequired, };

const SingleListItem = memo(WrappedSingleListItem);

// List Component const WrappedListComponent = ({ items }) => { const [selectedIndex, setSelectedIndex] = useState();

useEffect(() => { setSelectedIndex(null); }, [items]);

const handleClick = (index) => { // setSelectedIndex(index) selectedIndex === index ? setSelectedIndex(null) : setSelectedIndex(index); };

return ( <ul style={{ textAlign: "left" }}> {items.map((item, index) => ( <SingleListItem key={index} onClickHandler={() => handleClick(index)} text={item.text} index={index} isSelected={selectedIndex === index} /> ))} ); };

WrappedListComponent.propTypes = { items: PropTypes.arrayOf( PropTypes.shape({ text: PropTypes.string.isRequired, }) ), };

WrappedListComponent.defaultProps = { items: [{ text: "I" }, { text: "am" }, { text: "placed" }, { text: "hell" } ], };

const List = memo(WrappedListComponent);

export default List;