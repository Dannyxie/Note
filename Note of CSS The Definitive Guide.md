##CHAPTER 6 Text Properties 

###Indenting Text

	text-indent
	Values: <length> | <percentage> | inherit
	Initial value: 0
	Applies to: Block-level elements
	Inherited: Yes
	Percentages: Refer to the width of the containing block
	Computed value: For percentage values, as specified; for length values, the absolute length

- text-align can be applied to any block-level element, but it can't be applied to inline elements or on replaced elements such as images.
-  to “indent” the first line of an inline element, use left padding or margin.


###Horizontal Alignment
	text-align
	CSS2.1 values: left | center | right | justify | inherit
	CSS2 values: left | center | right | justify | <string> | inherit
	Initial value: User agent-specific; may also depend on writing direction
	Applies to: Block-level elements
	Inherited: Yes
	Computed value: As specified
	Note: CSS2 included a <string> value that was dropped from CSS2.1 due to
	a lack of implementation

![](http://i.imgur.com/yeGRrXZ.png)

- text-align does not control the alignment of elements, only their inline content. The actual elements are not shifted from one side to the other. Only the text within them is affected
