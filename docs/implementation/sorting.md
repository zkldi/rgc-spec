# RGC Sorting Specification

RGC has a well-defined sorting method. As mentioned in [parsers](../parsing),
parsers **SHOULD** sort their data after parsing.

Similarly, RGC files **SHOULD** be saved-as-sorted.

Regardless, neither can really be well-ensured, and the mistakes cause silent issues.

## RGC Consistent Event Sorting Algorithm (CESA)

The sorting algorithm is implemented **EXACTLY** as follows.

```js
// This algorithm, and more, come bundled with RGCUtils.
// Note that this is an in-place sorting algorithm,
function StableSortRGC(rgc) {
    for (const type in rgc.events) {
        rgc.events[type].sort((a, b) => {
            // if the ms's aren't identical, return in sorted order
            if (a.ms !== b.ms) {
                return a.ms - b.ms;
            }
            
            // else, we need to resolve stable sorted documents.
            return SubStableSortRGC(a, b);
        });
    }
}

function SubStableSortRGC(a, b) {
    // array checks to avoid undefined nonsense hits
    if (Array.isArray(a)) {
        let r = a.length - b.length;

        if (r !== 0) {
            return r;
        }
    }

    // get all the keys in lexicographical order
    // this is also well defined for array input
    let keys = Object.keys(a).sort();

    for (const key of keys) {
        let t = typeof a[key];

        let r = 0;

        if (t === "object") {
            // recurse
            r = SubStableSortRGC(a[key], b[key]);
        }
        else if (t === "number" || t === "boolean") {
            r = a[key] - b[key];
        }
        else if (t === "string") {
            r = a[key].localeCompare(b[key], "en");
        }
        else {
            throw new TypeError(`Unknown JSON type ${t} inside object!`);
        }

        // if the values were different
        if (r !== 0) {
            return r;
        }
    }

    // if we've got all the way here, these objects are identical
    return 0;
}
```

The reason for the above algorithm being so complex is such that we do not rely
on stable-sort behaviour for cases where objects share `ms`.

This is because this sorting is depended upon for the hashing algorithms, and therefore must
always produce *identical* output.

