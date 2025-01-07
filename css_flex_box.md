# CSS FlexBox:
---

- It is used to create proper layouts in webpages.

- First give <mark>display:flex</mark> property to the root element, this will change behaviour of all <strong> direct children </strong> elements inside that root element(flexible box).

### => FlexBox Properties:
---

- There are mainly 6 properties for flexbox:
	- flex-direction : row/column/reversed_versions
	- flex-wrap : wrap/nowrap
	- justify-content : start/center/end
	- align-items: start/center/end
	- align-content: start/center/end
	- flex-flow: (shorthand for flex-direction and flex-wrap)
	
- <strong>flex-direction :</strong> sets the main axis direction, by default its row (horizontal), thus every item will arrange horizontally side-by-side. The cross-axis is opposite of main axis.

- <strong>justify-content :</strong> it aligns items along the main axis.

- <strong>align-items :</strong> it aligns items along the cross axis.

- <strong>flex-wrap :</strong> if set to wrap then it wraps content.

- <strong>align-content :</strong> it works only **if flex-wrap is set to wrap and content is also wrapping**, it aligns items along cross axis.

- <strong>flex-flow :</strong> shorthand for flex-direction and flex-wrap, (we can also use gap prperty like this : gap:1em to set gap between children).

- <strong>gap :</strong> used to give the amount of space between flex-items.


### => FlexChildren Properties:
---

- These properties are for direct children of flexbox, there are mainly 5 properties:
	- **flex-grow** : unitless number that defines the ratio for how much the child will grow in given total space along main-axis, default is 0
	- **flex-shrink** : unitless number that defines the ratio for how much the child will shrink in given total space along main-axis, defalut is 0
	- **flex-bases** : size, it sets the initial main size of flex item, default is size of content
	- **flex** : shorthand for above 3 where last 2 are optional
	- **align-self** : override align-items property for that perticular child
