# Overview of class Date extension
```javascript
Date.prototype.ToDateString = function () {
    return this.ToString("yyyy-MM-dd");
}

Date.prototype.ToDateTimeString = function () {
    return this.ToString("yyyy-MM-dd hh:mm");
}

Date.prototype.ToString = function (format) {
    if (format) {
        var FillZero = function (n, len) {
            var l = Z.SafeToInt(len, 1);
            var str = n + String.Empty;
            while (str.length < l) {
                str = "0" + str;
            }
            return str;
        }

        var chinaNums = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九', '十', '十一', '十二'];
        var chinaMonths = ['一月', '二月', '三月', '四月', '五月', '六月', '七月', '八月', '九月', '十月', '十一月', '十二月'];
        var theDates = ['一', '二', '三', '四', '五', '六', '日']

        var year = this.getFullYear();

        if (isNaN(year)) {
            return String.Empty;
        }

        var syear = FillZero(year, 4);
        var month = this.getMonth();
        var day = this.getDate();
        var hour = this.getHours();
        var minute = this.getMinutes();
        var sec = this.getSeconds();
        var msec = this.getMilliseconds();
        var arr = [];
        var temp = String.Empty;
        for (var i = 0; i < format.length; i++) {
            var c = format.charAt(i);
            //小写
            var cc = c.toString().toLowerCase();
            var next = format.charAt(i + 1) || String.Empty;
            //小写
            var next2 = next.toString().toLowerCase();
            switch (c) {
                case 'Y':
                    temp += c;
                    if (next != 'Y') {
                        if (temp == 'Y' || temp == 'YY') {//一、二个y，后两位15年
                            var sy = syear.substr(2);
                            var y = Z.SafeToInt(sy);
                            var y1 = chinaNums[Z.SafeToInt(sy.charAt(0))];
                            var y2 = chinaNums[Z.SafeToInt(sy.charAt(1))];
                            if (temp == 'Y' && y < 10) {
                                arr.push(y1);
                            }
                            else {
                                arr.push(y1);
                                arr.push(y2);
                            }
                        }
                        else {
                            for (var j = 0; j < syear.length; j++) {
                                var sy = chinaNums[Z.SafeToInt(syear.charAt(j))];
                                arr.push(sy);
                            }
                        }
                        arr.push("年");
                        temp = String.Empty;
                    }
                    break;
                case 'y':
                    temp += c;
                    if (next != 'y') {
                        if (temp == 'y' || temp == 'yy') {//一、二个y，后两位15年
                            arr.push(syear.substr(2));
                        }
                        else {
                            arr.push(syear);
                        }
                        temp = String.Empty;
                    }
                    break;
                case 'M': //Month
                    temp += c;
                    if (next != 'M') {
                        var m = month + 1;
                        if (temp == 'M') {
                            arr.push(m);
                        }
                        else if (temp == 'MM') {
                            arr.push(FillZero(m, 2));
                        }
                        else {
                            arr.push(chinaMonths[month]);
                        }
                        temp = String.Empty;
                    }
                    break;
                case 'd': //Day
                    temp += c;
                    if (next != 'd') {
                        var d = day;
                        if (temp == 'd') {
                            arr.push(d);
                        }
                        else if (temp == 'dd') {
                            arr.push(FillZero(d, 2));
                        }
                        else if (temp == 'ddd') {
                            arr.push('周' + theDates[this.getDay() - 1]);
                        }
                        else {
                            arr.push('星期' + theDates[this.getDay() - 1]);
                        }
                        temp = String.Empty;
                    }
                    break;
                case 'h':
                case 'H': //Hour
                    temp += cc;
                    if (next2 != 'h') {
                        if (temp == 'h') {//一个h
                            arr.push(hour);
                        }
                        else {
                            arr.push(FillZero(hour, 2));
                        }
                        temp = String.Empty;
                    }
                    break;
                case 'm': //Minute
                    temp += cc;
                    if (next != 'm') {
                        if (temp == 'm') {//一个m
                            arr.push(minute);
                        }
                        else {
                            arr.push(FillZero(minute, 2));
                        }
                        temp = String.Empty;
                    }
                    break;
                case 's': //second
                    temp += cc;
                    if (next != 's') {
                        if (temp == 's') {//一个s
                            arr.push(sec);
                        }
                        else {
                            arr.push(FillZero(sec, 2));
                        }
                        temp = String.Empty;
                    }
                    break;
                default:
                    arr.push(c);
                    temp = String.Empty;
            }
        }
        return arr.join(String.Empty);
    }
    return this.ToString("yyyy-MM-dd hh:mm:ss");
}

Date.prototype.ToShortDateString = function () {
    return this.ToDateString();
}

Date.prototype.ToLongDateString = function () {
    return this.ToString("yyyy年MM月dd日");
}

Date.prototype.AddMinutes = function (m) {
    this.setMinutes(this.getMinutes() + m);
    return this;
}

Date.prototype.AddHours = function (h) {
    this.setHours(this.getHours() + h);
    return this;
}

Date.prototype.AddDays = function (d) {
    this.setDate(this.getDate() + d);
    return this;
}

Date.prototype.AddMonths = function (m) {
    this.setMonth(this.getMonth() + m);
    return this;
}

Date.prototype.AddYears = function (y) {
    this.setFullYear(this.getFullYear() + y);
    return this;
}

Date.prototype.GetLastDay = function () {
    var dt = new Date(this.getFullYear(), this.getMonth() + 1, 1);

    return dt.AddDays(-1).getDate();
}

```
