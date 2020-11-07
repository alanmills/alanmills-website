# In Vim how to convert a line of text to lower case and replace whitespace with hyphens
**Author:** [Alan Mills]
**Date:** [29 January 2018 08:27]
**Tags:** [Vim]
**Status**: Draft

In Vim command mode, navigate to the line of text you want to format and then:
1. Convert the line to lowercase `gu`
2. Replace the whitespace to hyphens `:s/\s/-/g`
