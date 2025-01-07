# CSS Units
---

- There are two types of css units:
	- 1.) Absolute 2.) Relative
	
- The relative units are either relative to font size or relative to the size of the view port.

- the **px** is the smallest dimenstion for any screen, its the most used absolute unit.

- **16 px** is the default font size for all browsers.

- The **em** unit is relative to the font size of the element it is assigned to, unless it is assigned to the font-size property in which case it will be relative to the font size of the parent.

- the **rem** unit is relative to the root element, the root element is the html element which has default size of 16 px set to browsers.

- Conclusion: use rem unit for fonts, us em unit for paddings, borders, margins, widths etc... which will be relative to the font size of its own element.

- Percentages are relative to their parent.

- the **vw** and **vh** units are relative to viewport width and viewport height.

- **The vh/vw don't gets effect of zooming in or zooming out**

- the **ch** unit measures the width of the charecter 0 on the font, it can be used to set max-width of a paregraph. for example if we set the max-width: 60ch to a paregraph then it will have maximum 60 charecters in it where size of 1 charecter is the font size of that paregraph.

---

### CSS variables:
---

```
:root{
	--color_dark: black;
}

body {
	background-color: var(--color_dark)
}
```

---

## CSS mediaquery and responsiveness
---

- if you design website for desktop-first then use max-width to specify media query, and use min-width to specify media query for mobile first websites.

- When you zoom-in/out the font size changes thus where ever the em unit are used will be changed as well, however the vh/vw units don't change on zoom because ther are relative to "viewport" means they are realtive to what you are seeing!

- the default width and height of an element depends on its content, if the width and height is not specified then zooming-in/out will change both dimensions.

- do not mainly focus on behaviour of zooming ,instead focus on the changes of widht, thus use media query.  
