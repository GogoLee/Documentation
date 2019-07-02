# Overview of class Number extension
```javascript
Number.prototype.ToString = function (format) {
    if (format) {

        if (format.indexOf("#,") == -1 && format.indexOf(".") == -1) {
            var rd = format.length;
            return Helper.FillZero(this, rd);
        }

        var formats = format.split(".");

        var rc = formats.length == 2 ? formats[1].length : 0; //小数位

        var num = this.Round(rc);

        var str = num.toString();
        var strs = str.split(".");
        var str1 = strs[0];
        var str2 = String.Empty;
        //补充小数位数（补零）
        if (rc > 0) {
            if (strs.length == 2) {
                str2 = strs[1];
            }
            var zeroCount = rc - str2.length;
            while (zeroCount-- > 0) {
                str2 = str2 + "0";
            }
            str = str1 + "." + str2;
        }
        var ft = formats[0];
        //货币类型，补充“,”号
        if (ft == "#,0") {
            var results = str1;
            if (str1.length > 3) {
                var stack = [];
                var len = str1.length; //小数点前字符串长度
                for (var i = 0; i < len; i++) {
                    var ch = str1.charAt(i);
                    stack.push(ch);
                    var c = len - i - 1; //倒数第几个字符
                    if ((c % 3) == 0 && c != 0 && c != len) {//每3位补一个“,”                    
                        if (ch != '-') {//负数
                            stack.push(",");
                        }
                    }
                }
                results = stack.join(String.Empty);
            }
            if (rc > 0) {
                results = results + "." + str2;
            }
            return results;
        }
        return str;
    }
    return this.toString();
}

Number.prototype.Round = function (e) {
    var t = 1;
    for (; e > 0; t *= 10, e--);
    for (; e < 0; t /= 10, e++);
    return Math.round(this * t) / t;
}

```
