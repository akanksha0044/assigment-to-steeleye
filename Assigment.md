1. Explain what the simple `List` component does.

List Component display an unordered List with some texts as List items. It takes an array (items) of strings(text) as props. For each items in the array, it maps through all and displays their content on the screen with a background color of 'Red'. When the user will click on single item, it will change to different color i.e Green.


2. What problems / warnings are there with code?

i) syntax error in UseState
```javascript
// error code
const [setSelectedIndex, selectedIndex] = useState();
```
```javascript
// updated code
const [selectedIndex, setSelectedIndex] = useState();
```
ii) There should be function reference instead of call
```javascript
// error code
onClick={onClickHandler(index)} 
```
```javascript
// updated code
onClick={() => onClickHandler(index)}
```
 
iii) It should be `arrayOf` and `shape` instead of `array` and `shapeOf`

```javascript
//error code
 WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};
```
```javascript
//updated code
WrappedListComponent.propTypes = {
items: PropTypes.arrayOf(
PropTypes.shape({
text: PropTypes.string.isRequired,
       })
    ),
};
```

iv) It is not possible to map a null array 
```javascript
//error code
WrappedListComponent.defaultProps = {
  items: null,
};
```
```javascript
//updated code
WrappedListComponent.defaultProps = {
items: [
            {text: "Akanksha", index:1},
            {text: "Akanksha2", index: 2}
           ],
};
```
                OR

Alternate way if we don't to store any value
```javascript
//updated code
WrappedListComponent.defaultProps = {
    // items: null //giving null as default prop is never recommended
    items: undefined 
};
```
v) Unique key is missing
```javascript
//error code
<ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
```
```javascript
//updated code
        <ul style={{ textAlign: 'left' }}>
            {items.map((item, index) => (
                <SingleListItem
                    key={index}
                    onClickHandler={() => handleClick(index)}
                    text={item.text}
                    index={index}
                    isSelected={selectedIndex}
                />
            ))}
     </ul>
```

3. Please fix, optimize, and/or modify the component as much as you think is necessary.

```javascript
//error code

import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
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
const WrappedListComponent = ({
  items,
}) => {
  const [setSelectedIndex, selectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items: null,
};

const List = memo(WrappedListComponent);

export default List;
```

```javascript
//updated code

import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
    index,
    isSelected,
    onClickHandler,
    text,
}) => {
    return (
        <li
            style={{ backgroundColor: isSelected ? 'green' : 'red' }}
            onClick={()=>onClickHandler(index)}
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
const WrappedListComponent = ({
    items,
}) => {
    const [selectedIndex, setSelectedIndex] = useState();

    useEffect(() => {
        setSelectedIndex(null);
    }, [items]);

    const handleClick = index => {
        setSelectedIndex(index);
    };

    return (
        <ul style={{ textAlign: 'left' }}>
            {items.map((item, index) => (
                <SingleListItem
                    key={index}
                    onClickHandler={() => handleClick(index)}
                    text={item.text}
                    index={index}
                    isSelected={selectedIndex}
                />
            ))}
        </ul>
    )
};

WrappedListComponent.propTypes = {
    items: PropTypes.arrayOf(PropTypes.shape({
        text: PropTypes.string.isRequired,
    })),
};

WrappedListComponent.defaultProps = {
    items: [{ text: "Akanksha", index: 1 },
    { text: "Akanksha2", index: 2 }]
};

const List = memo(WrappedListComponent);

export default List;
```
