ThingDoc is a clever things comment parser.

It uses syntax similar to Javadoc and is able to generate:

  * BoM text file
  * HTML documentation
  * TeX documentation
  * Wiki documentation (suitable for the reprap.org wiki)
  * Dependency tree in text-mode and using GraphViz

For more information check our wiki at: http://thingdoc.org/

# Marking Up Your Design with ThingDoc

When run, ThingDoc automatically parses text files with .scad and .tdoc extensions,
looking for its control keywords, detailed below.

Control keywords must be contained within a comment block with the following formatting

```
/**
 * @name Bolt
 * @id m3
 */
```

The first line of the block must have both asterisks, the remaining lines must 
have a space before the asterisk, you can not have an empty line in the block,
the last line must contain a slash after the asterisk, and you may only enter
one part per code block.

## Common keywords

* @root - This keyword must appear only once in the parsed files, and it indicates the top most level of the part or assembly
* @id -  Mandatory keyword to assign a unique id to a part
* @name - The name of a part
* @desc - Description of a part
* @image - An image of a part, given as a filename with an extension

## Less common keywords

* @common
* @since - can be YYYY-MM-DD or some tag, e.g. "Mendel")
* @category - e.g. RP, fastener, etc
* @step
* @comment
* @using - dependencies on other parts
* @assembled
* @type - more detailed than category

# Command Line Usage

## File options	
	
* -i, --indir - start scanning in INDIR directory (current by default)
* -o, --outdir - use OUTDIR as output directory ("docs" by default)
* --imagedir - use IMAGEDIR directory (relative to OUTDIR) to look for images used in HTML and TeX ("images" by default)
	
## Validation options

* -l, --lint - check syntax in FILE and exit
* -x, --parse-only -
		
## Output options

* -b, --bom - generate Bill of Materials
* -m, --html - generate HTML (markup) documentation
* -t, --tex - generate TeX documentation
* -w, --wiki - generate Wiki documentation
* -p, --print - print tree of things and exit (text mode)
* -g, --graph - generate graphviz document

Note: If none of --bom, --html, --tex, --wiki are provided then all 4 types are generated.

# Examples

## Example 1

### File 1

```
/**
 * @root
 * @name Bolt Assembly
 * @using m3bolt
 * @using m3nut
 * @using washerAssy
 * @assembled
 */

/**
 * @name M3x15mm Socket Head Bolt
 * @common
 * @category Vitamins
 * @type bolt
 * @id m3bolt
 */

/**
 * @name M3 Hex Nut
 * @common
 * @category Vitamins
 * @type nut
 * @id m3nut
 */
```

### File 2

```
/**
 * @name Washer Assembly
 * @category Vitamins
 * @using m3washer
 * @using m3washer
 * @using m3lockWasher
 * @id washerAssy
 */
 
/**
 * @name M3 Washer
 * @category Vitamins
 * @type washer
 * @id m3washer
 */
 
/**
 * @name M3 Lock Washer
 * @category Vitamins
 * @type washer
 * @id m3lockWasher
 */
```

### Text BOM Output

```
Bill of Materials: Bolt Assembly
++++++++++++++++++++++++++++++++

Vitamins
========
- 1x M3 Hex Nut
- 2x M3 Washer
- 1x M3x15mm Socket Head Bolt
- 1x M3 Lock Washer
- 1x Washer Assembly

----
Generated on 2013/10/24 22:47:28 by ThingDoc - https://github.com/josefprusa/ThingDoc/wiki
```

## Example 2

