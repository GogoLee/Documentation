## Overview of class String extension
```javascript
String.prototype.Concat = function (s) {
    var arr = [];
    if (arguments && arguments.length > 0) {
        for (var i = 0; i < arguments.length; i++) {
            arr.push(arguments[i]);
        }
    }
    return arr.join(String.Empty);
}

String.prototype.Equals = function (s, ignoreCase) {
    var ss = s;
    var _this = this;
    if (ignoreCase) {
        ss = ss.toLowerCase()
        _this = _this.toLowerCase();
    }
    return ss == _this;
}

String.prototype.Join = function (separator, strs) {
    if (arguments && arguments.length > 1) {
        var arr = [];
        for (var i = 1; i < arguments.length; i++) {
            arr.push(arguments[i]);
        }
        return arr.join(separator);
    }
    return String.Empty;
}
String.prototype.Contains = function (s, ignoreCase) {
    if (!ignoreCase) {
        return this.indexOf(s) > -1;
    }
    var ss = this.toLowerCase();
    s = s.toLowerCase();
    return ss.indexOf(s) > -1;
}

String.prototype.Split = function (separator) {
    if (!separator) {
        return [this];
    }
    var separators = [];
    if (separator instanceof String) {
        separators.push(arr);
    }
    if (separator instanceof Array) {
        separators = separator;
    }

    var temp = this;
    for (var i = 0; i < separators.length; i++) {
        var sep = separators[i];
        if (sep != ",") {
            temp = temp.Replace(sep, ",");
        }
    }
    return temp.split(",");
}

String.prototype.EndsWith = function (s, ignoreCase) {
    if (s) {
        var len = s.length;
        if (len == 0) {
            return true;
        }
        if (this.length >= len) {
            while (len > 0) {
                len--;
                var c1 = this.charAt(this.length - (s.length - len));
                var c2 = s.charAt(len);
                if (ignoreCase) {
                    c1 = c1.toString().toLowerCase();
                    c2 = c2.toString().toLowerCase();
                }
                if (c1 != c2) {
                    return false;
                }
            }
            return true;
        }
    }
    return false;
}

String.prototype.Replace = function (oldValue, newValue) {
    return this.replace(new RegExp(oldValue, "g"), newValue || String.Empty);
}

String.prototype.TrimHelper = function (chars, trimType) {
    var trimChars = [];
    if (chars && chars.length > 0) {
        for (var i = 0; i < chars.length; i++) {
            trimChars.push(chars[i]);
        }
    }
    var end = this.length - 1;
    var start = 0;
    if (trimType != 1) {
        start = 0;
        while (start < this.length) {
            var ch = this.charAt(start);
            if (trimChars != null && trimChars.length > 0) {
                var index = 0;
                while (index < trimChars.length) {
                    if (trimChars[index] == ch) {
                        break;
                    }
                    index++;
                }
                if (index == trimChars.length) {
                    break;
                }
                start++;
            }
            else {
                if (ch != ' ') {
                    break;
                }
                start++;
            }
        }
    }
    if (trimType != 0) {
        end = this.length - 1;
        while (end >= start) {
            var ch2 = this.charAt(end);
            if (trimChars != null && trimChars.length > 0) {
                var index = 0;
                while (index < trimChars.length) {
                    if (trimChars[index] == ch2) {
                        break;
                    }
                    index++;
                }
                if (index == trimChars.length) {
                    break;
                }
                end--;
            }
            else {
                if (ch2 != ' ') {
                    break;
                }
                end--;
            }
        }
    }
    return this.slice(start, end + 1);
}

String.prototype.Trim = function (chars) {
    return this.TrimHelper(arguments, 2);
}

String.prototype.TrimStart = function (chars) {
    return this.TrimHelper(arguments, 0);
}

String.prototype.TrimEnd = function (chars) {
    return this.TrimHelper(arguments, 1);
}

```
