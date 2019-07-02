# Overview of class Array extension
```javascript
Array.prototype.Contains = function (o) {
    var len = this.length;
    for (var i = 0; i < len; i++) {
        if (typeof o == 'function') {
            if (o(this[i])) {
                return true;
            }
        }
        else {
            if (this[i] === o) {
                return true;
            }
        }
    }
    return false;
}

Array.prototype.IndexOf = function (o) {
    var len = this.length;
    for (var i = 0; i < len; i++) {
        if (this[i] === o) {
            return i;
        }
    }
    return -1;
}

Array.prototype.Count = function (o) {
    var len = this.length;
    if (typeof o == 'function') {
        var c = 0;
        for (var i = 0; i < len; i++) {
            if (o(this[i])) {
                c++;
            }
        }
        return c;
    }
    if (typeof o != 'undefined') {
        var c = 0;
        for (var i = 0; i < len; i++) {
            if (this[i] === o) {
                c++;
            }
        }
        return c;
    }
    return len;
}

Array.prototype.Find = function (fn) {
    var len = this.length;
    if (typeof fn == 'function') {
        var arr = [];
        for (var i = 0; i < len; i++) {
            var o = this[i];
            if (fn(o)) {
                arr.push(o);
            }
        }
        return arr;
    }
    return this;
}

Array.prototype.Where = Array.prototype.Find;

Array.prototype.Exist = function (fn) {
    var len = this.length;
    var isExist = false;
    if (typeof fn == 'function') {
        for (var i = 0; i < len; i++) {
            var o = this[i];
            if (fn(o)) {
                isExist = true;
                break;
            }
        }
    }
    return isExist;
}

Array.prototype.First = function (fn) {
    var len = this.length;
    if (typeof fn != 'function') {
        if (len > 0) {
            return this[0];
        }
    }
    for (var i = 0; i < len; i++) {
        var o = this[i];
        if (fn(o)) {
            return o;
        }
    }
    return null;
}

Array.prototype.Remove = function (o) {
    var i = this.IndexOf(o);
    if (i > -1) {
        this.splice(i, 1);
    }
    return this;
}

Array.prototype.RemoveAll = function (o) {
    var len = this.length;
    for (var i = 0; i < len; i++) {
        if (this[i] == o) {
            this.splice(i, 1);
            i--;
            len--;
        }
    }
    return this;
}

Array.prototype.RemoveWhile = function (fn) {
    var len = this.length;
    for (var i = 0; i < len; i++) {
        if (fn(this[i]) === true) {
            this.splice(i, 1);
            i--;
            len--;
        }
    }
    return this;
}

Array.prototype.RemoveRange = function (start, end) {
    this.splice(start, end - start);
    return this;
}

Array.prototype.Clear = function () {
    this.splice(0, this.length);
    return this;
}

Array.prototype.Select = function (converter, filter) {
    var len = this.length;
    if (typeof converter == 'function') {
        var arr = [];
        for (var i = 0; i < len; i++) {
            var o = this[i];
            if (typeof filter != 'function' || filter(o)) {
                arr.push(converter(o));
            }
        }
        return arr;
    }
    return this;
}

Array.prototype.RemoveAt = function (index) {
    if (index < this.length) {
        this.splice(index, 1);
        return true;
    }
    return false;
}
Array.prototype.Insert = function (index, o) {
    if (index <= this.length) {
        this.splice(index, 0, o);
        return this;
    }
    return false;
}

Array.prototype.MoveUp = function (o) {

}

Array.prototype.OrderBy = function (fn) {
    /// <summary>升序(小->大)</summary>
    var list = new Array().concat(this);
    var n = this.length;
    if (n > 1) {
        for (var i = 0; i < n; i++) {
            var flag = false;
            for (var j = 0; j < n - i - 1; j++) {
                var v1 = list[j];
                var v2 = list[j + 1];
                if (typeof fn == 'function') {
                    v1 = fn(v1);
                    v2 = fn(v2);
                }
                if (v2 < v1) {
                    var temp = list[j];
                    list[j] = list[j + 1];
                    list[j + 1] = temp;
                    flag = true;
                }
            }
            if (!flag)
                break;
        }
    }
    return list;
}

Array.prototype.OrderByDesc = function (fn) {
    /// <summary>降序(大->小)</summary>
    var list = new Array().concat(this);
    var n = this.length;
    if (n > 1) {
        for (var i = 0; i < n; i++) {
            var flag = false;
            for (var j = 0; j < n - i - 1; j++) {
                var v1 = list[j];
                var v2 = list[j + 1];
                if (typeof fn == 'function') {
                    v1 = fn(v1);
                    v2 = fn(v2);
                }
                if (v2 > v1) {
                    var temp = list[j];
                    list[j] = list[j + 1];
                    list[j + 1] = temp;
                    flag = true;
                }
            }
            if (!flag)
                break;
        }
    }
    return list;
}

Array.prototype.GroupBy = function (fn) {
    var results = [];
    var n = this.length;
    if (n > 0 && $.isFunction(fn)) {
        for (var i = 0; i < n; i++) {
            var it = this[i];
            var key = fn(it);
            var g = results.First(function (o) { return o.Key === key; });
            if (!g) {
                g = { Key: key, Items: [] };
                results.push(g);
            }
            g.Items.push(it);
        }
    }
    return results;
}

Array.prototype.Max = function (fn, converter) {
    var len = this.length;
    if (len > 0) {
        if (typeof fn == 'function') {
            var temp = fn.call(this, this[0]);
            var result = temp;
            if ($.isFunction(converter)) {
                result = converter.call(this, this[0]);
            }
            for (var i = 1; i < len; i++) {
                var next = fn.call(this, this[i]);
                var nextResult = next;
                temp = ((temp > next) ? temp : next);
                if ($.isFunction(converter)) {
                    nextResult = converter.call(this, this[i]);
                }
                result = ((temp > next) ? result : nextResult);
            }
            return result;
        }
        if (typeof fn == 'undefined') {
            return this.Max(function (o) { return o; });
        }
    }
}

Array.prototype.Min = function (fn, converter) {
    var len = this.length;
    if (len > 0) {
        if (typeof fn == 'function') {
            var temp = fn.call(this, this[0]);
            var result = temp;
            if ($.isFunction(converter)) {
                result = converter.call(this, this[0]);
            }
            for (var i = 1; i < len; i++) {
                var next = fn.call(this, this[i]);
                var nextResult = next;
                temp = ((temp < next) ? temp : next);
                if ($.isFunction(converter)) {
                    nextResult = converter.call(this, this[i]);
                }
                result = ((temp < next) ? result : nextResult);
            }
            return result;
        }
        if (typeof fn == 'undefined') {
            return this.Min(function (o) { return o; });
        }
    }
}

Array.prototype.Sum = function (fn) {
    var len = this.length;
    if (len > 0) {
        if (typeof fn == 'function') {
            var sum = fn.call(this, this[0]);
            for (var i = 1; i < len; i++) {
                sum = (sum + fn.call(this, this[i]));
            }
            return sum;
        }
        if (typeof fn == 'undefined') {
            return this.Sum(function (o) { return o; });
        }
    }
    return 0;
}
Array.prototype.Average = function (fn) {
    var len = this.length;
    if (len > 0) {
        var sum = this.Sum(fn);
        return sum / len;
    }
    return 0;
}
Array.prototype.Distinct = function (fn) {
    var len = this.length;
    if (len > 0) {
        var results = [];
        if (typeof fn == 'function') {
            for (var i = 1; i < len; i++) {
                var item = fn.call(this, this[i]);
                if (!results.Contains(item)) {
                    results.push(item);
                }
            }
            return results;
        }
        if (typeof fn == 'undefined') {
            return this.Distinct(function (o) { return o; });
        }
    }
    return this;
}

Array.prototype.AddOrUpdate = function (o, comparer) {
    var isUpdated = false;
    for (var i = 0; i < this.length; i++) {
        var x = this[i];
        if (($.isFunction(comparer) && comparer(x)) || (comparer === undefined && x === o)) {
            this[i] = o;
            isUpdated = true;
            break;
        }
    }
    if (!isUpdated) {
        this.push(o);
    }
}

Array.Create = function (length, defValue) {
    var len = length || 0;
    var arr = new Array(len);
    if (defValue !== undefined) {
        for (var i = 0; i < arr.length; i++) {
            arr[i] = defValue;
        }
    }
    return arr;
}

Array.prototype.Take = function (start, end) {
    var arr = [];
    if (start >= 0 && end >= start && this.length > start) {
        for (var i = start; i < end && i < this.length; i++) {
            arr.push(this[i]);
        }
    }
    return arr;
}

Array.prototype.Each = function (fn) {
    if (typeof (fn) != "function") return;
    for (var i = 0; i < this.length; i++) {
        fn.apply(this[i], [this[i], i]);
    }
}

Array.prototype.Add = function (items) {
    if (!$.isArray(items)) items = [items];
    for (var i = 0; i < items.length; i++) {
        this.push(items[i]);
    }
}
```
