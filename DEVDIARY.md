# Development diary

`json_merge` = Combining two or more JSON files into a single one, by union
or merging of them as dictionaries.

Let's consider two JSON files representing some kind of collections, with a name
collision in one of the first-level
keys:

    ./merge/collection_one.json:
    {
        Functions: {
            f_one: "start()",
            f_two: "end()"
        },
        Vars: {
            one: 1,
            two: 2,
            three: 3
            four: 4
        }
    }
    ./merge/collection_two.json:
    {
        Streams: {
            "input": "yes",
            "output": "yes"
        },
        Vars: {
            one: 1,
            two: 2,
            three: "three",
        }
    }

The `Vars` name is expanded in the merged JSON file.
**Colliding values are taken from the latest file in the merge.**

    ./merged.json:
    {
        
        Functions: {
            f_one: "start()",
            f_two: "end()"
        }
        Vars: {
            one: 1,
            two: 2,
            three: "three",
            four: 4
        }
        Streams: {
            "input": "yes",
            "output": "yes"
        }
    }

Notice how `Vars.three` has a value of `"three"` in `merged.json`, which is
taken from the `./merge/collection_two.json` file, because it is processed
last; but `Vars.four` retains its original value of `4` from `collection_one.
json`.

**The challenge is to enter the nested JSON structure when a key collides, and
extend it.**

**Note: this must be repeated for inner nests with key collisions.**
