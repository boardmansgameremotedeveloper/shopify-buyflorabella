rescan templates/index.json.  update CLAUDE.md that this should be done in case of any changes inside shopify that need to be checked for every iteration.  duplicate the native multirow.liquid code for a custom birdlabs-multirow.liquid file.  the code should be identical, we will update in a future iteration.

create a second version of the birdlabs-multicolumn.liquid file which will have the same functionality and add additional areas to this new custom section:

1) an area above the feature cards which will have an additional single row with up to six images.  These areas area all meant to display chemistry information: the symbol of an element such as 'Mg' for Magnesium.  A small area underneat that with the element name such as "Magnesium" and in the upper-right area the number for the element such as '12' for Magnesium.  The idea is to create cards which look like a periodic table.

2) the feature cards for this section should be rounded with a white background.  There is a small rounded corner box inside the feature card with the chemistry symbol for the element, e.g. "Mg" for magnesium.  There there is the element name, and a short description of the benefit of this element just underneath the element name.

3) Underneath all of the feature cards is a dropdown selector which shows "Guaranteed Analysis" with a down arrow.  When it drops down it shows a styled table with: the element symbol on the left, the element name next to it, and on the far right a percentage % of how much of this element is in the product.  There is a small line visual divider at the bottom of the area which "appears as if" it is a dropdown selector, although it is really only revealing a new content area.  Underneath this divider is short descriptive text.

4) Underneath this area is additional descriptive text.

5) underneath this area is an additional area to add "element" cards to which are tiny rounded corner boxes with a dropshadow, and the elemenent Symbol appearing on each card.  We may want to make this something like a .csv list of element symbols, since there will be more than 10 and putting that into the shopify CMS may be awkward for individual symbols.  Figure out the best approach.

