---
title: mypub
date: 2018-05-02 08:09:47
tags:
---

常用函数

<!-- more -->

```js
import { timingSafeEqual } from 'crypto';

var moment = require('moment');
var util   = require('util');
var dbhd   = require('./dboper');
var basic  = require('./basic');
var mqhd   = require('./mqoper');
require('./errorRecord');

const ONE_MINUTE_MILL_SECONDS = 1000 * 60;
const ONE_HOUR_MILL_SECONDS   = ONE_MINUTE_MILL_SECONDS * 60;
const ONE_DAY_MILL_SECONDS    = ONE_HOUR_MILL_SECONDS * 24;
const ONE_WEEK_MILL_SECONDS   = ONE_DAY_MILL_SECONDS * 7;
const ONE_YEAR_MILL_SECONDS   = ONE_DAY_MILL_SECONDS * 365;

const ERR_RESULT_SUCCESS     = "success";
const ERR_RESULT_FAIL        = "fail";
const ERR_RESULT_BIG_SUCCESS = "SUCCESS";
const ERR_RESULT_BIG_FAIL    = "FAIL";

var ERR_CODE_PARAM = 1;
var ERR_CODE_PUB   = 2;

function json2str(jsonData) {
    return JSON.stringify(jsonData);
}

function print(data) {
    return util.inspect(data, { depth: null });
}

function getZeroTimeNumYesterday() {
    var dateTime = new Date();
    var zeroTimeYesterday = new Date(dateTime.toDateString()) - ONE_DAY_MILL_SECONDS;

    return zeroTimeYesterday;
}

function getZeroTimeNumToday() {
    var dateTime = new Date();
    var zeroTimeToday = +new Date(dateTime.toDateString());

    return zeroTimeToday;
}

function isNum(param) {
    return Object.prototype.toString.call(param) === "[object Number]";
}

function isString(param) {
    return Object.prototype.toString.call(param) === "[object String]";
}

function isUndefined(param) {
    return Object.prototype.toString.call(param) === "[object Undefined]";
}

function isBoolean(param) {
    return Object.prototype.toString.call(param) === "[object Boolean]";
}

function isObject(param) {
    return Object.prototype.toString.call(param) === "[object Object]";
}

function isArray(param) {
    return Object.prototype.toString.call(param) === "[object Array]";
}

function isFunction(param) {
    return Object.prototype.toString.call(param) === "[object Function]";
}

function indexOf(array, val) {
    for (var i = 0; i < array.length; i++) {
        if (array[i] == val) {
            return i;
        }
    }
    return -1;
}

function removeArrya(array, val) {
    var index = indexOf(array, val);
    if (-1 != index) {
        array.splice(index, 1);
    }
    return array;
}

function removeArrSpecifyObj(array, key, val) {
    array.map(
        function(item, index) {
            var flag = item[key] == val ? true : false;
            if (true == flag) {
                array.splice(index, 1);
            }
        }
    );
    return array;
}

function uniqueArray(array) {
    var obj = {};
    var res = [];
    array.forEach(function(item) {
        if (!obj[item]) {
            obj[item] = true;
            res.push(item);
        }
    });
    return res;
}

function compare(sortName) {
    return function(o, p) {
        var a = {};
        var b = {};
        if (isObject(o) && isObject(p) && o && p) {
            a = o[sortName];
            b = p[sortName];
        }
        if (a === b) {
            return 0;
        }
        if (typeof a === typeof b) {
            return (a < b) ? -1 : 1;
        }
        return (typeof a < typeof b) ? -1 : 1;
    }
}

function stampToday() {
    var today = moment().startOf("day");
    var timestamp = [];
    timestamp.push(today.format("YYYY-MM-DD HH:00:00"));
    for (var i = 0; i < 24; i++) {
        timestamp.push(today.add(1, "hours").format("YYYY-MM-DD HH:00:00"));
    }
    return timestamp;
}

function stampLast1Day() {
    var from = moment().subtract(1, "days");
    var timestamp = [];
    timestamp.push(from.format("YYYY-MM-DD HH:00:00"));
    for (var i = 0; i < 24; i++) {
        timestamp.push(from.add(1, "hours").format("YYYY-MM-DD HH:00:00"));
    }
    return timestamp;
}

function stampLastNumDays(dayNum) {
    var from = moment().subtract(dayNum, "days");
    var timestamp = [];
    timestamp.push(from.format("YYYY-MM-DD"));
    for (var i = 0; i < dayNum; i++) {
        timestamp.push(from.add(1, "days").format("YYYY-MM-DD"));
    }
    return timestamp;
}

function stampLast1Year() {
    var from = moment().subtract(1, "years");
    var timestamp = [];
    timestamp.push(from.format("YYYY-MM"));
    for (var i = 0; i < 12; i++) {
        timestamp.push(from.add(1, "months").format("YYYY-MM"));
    }
    return timestamp;
}

function stampThisHour(step) {
    var from = moment().startOf("hour");
    var timestamp = [];
    var count = 60 / step;
    timestamp.push(from.format("YYYY-MM-DD HH:mm:00"));
    for (var i = 0; i < count; i++) {
        timestamp.push(from.add(step, "minutes").format("YYYY-MM-DD HH:mm:00"));
    }
    return timestamp;
}

function stampLast1Hour(step) {
    var from = moment().subtract(1, "hour");
    var timestamp = [];
    var count = 60 / step;
    timestamp.push(from.format("YYYY-MM-DD HH:mm:00"));
    for (var i = 0; i < count; i++) {
        timestamp.push(from.add(step, "minutes").format("YYYY-MM-DD HH:mm:00"));
    }
    return timestamp;
}

function stampBeforeDays(dayBefore, daysNum) {
    var talBefore = dayBefore + daysNum;
    var from = moment().subtract(talBefore, "days");
    var timestamp = [];
    timestamp.push(from.format("YYYY-MM-DD"));
    for (var i = 1; i < daysNum; i++) {
        timestamp.push(from.add(1, "days").format("YYYY-MM-DD"));
    }
    return timestamp;
}

function stampBeforeHours(hoursBefore, hoursNum) {
    var talBefore = hoursBefore + hoursNum;
    var from = moment().subtract(talBefore, "hours");
    var timestamp = [];
    timestamp.push(from.format("YYYY-MM-DD HH"));
    for (var i = 1; i < hoursNum; i++) {
        timestamp.push(from.add(1, "days").format("YYYY-MM-DD HH"));
    }
    return timestamp;
}

function stampYesterday() {
    var nCurHour = (new Date()).getHours();
    return stampBeforeHours(Number(nCurHour), 24);
}

function lastHours(hours) {
    var from = moment().subtract(hours, "hours");
    var timestamp = [];
    for (var i = 0; i < hours; i++) {
        timestamp.push(from.add(1, "hours").format("YYYY-MM-DD HH:00:00"));
    }
    return timestamp;
}

function lastMinutess(mins) {
    var from = moment().subtract(mins, "minutes");
    var timestamp = [];
    for (var i = 0; i < mins; i++) {
        timestamp.push(from.add(1, "minutes").format("YYYY-MM-DD HH:mms:00"));
    }
    return timestamp;
}

function getStamps(timeId) {
    var times = [];
    switch (timeId) {
        case "thisHour": {
            times = stampThisHour(5);
            break;
        }
        case "lastHour": {
            times = stampLast1Hour(5);
            break;
        }
        case "lastDay": {
            times = stampLast1Day();
            break;
        }
        case "yesterday": {
            times = stampYesterday();
            break;
        }
        case "today": {
            times = stampToday();
            break;
        }
        case "last7Days": {
            times = stampLastNumDays(7);
            break;
        }
        case "last15Days": {
            times = stampLastNumDays(15);
            break;
        }
        case "last30Days": {
            times = stampLastNumDays(30);
            break;
        }
        case "lastYear": {
            times = stampLast1Year();
            break;
        }
        default: {
            console.error("error");
        }
        return times;
    }
}

function successV1(oData, key1, value1, key2, value2) {
    var oResponse = {
        result: ERR_RESULT_BIG_SUCCESS
    };
    if (null != oData) {
        oResponse.message = oData;
    }
    if (null != key1 && null != value1) {
        oResponse[key1] = value1;
    }
    if (null != key2 && null != value2) {
        oResponse[key2] = value2;
    }
    return oResponse;
}

function failV1(sFileName, nLineNum, sReason, nErrorCode) {
    var oResponse = {
        result: ERR_RESULT_BIG_FAIL
    };
    if (null != sFileName || null != nLineNum || null != sReason || null != nErrorCode) {
        oResponse.message = {};
    }
    if (null != sFileName) {
        oResponse.message.errorFile = sFileName;
    }
    if (null != nLineNum) {
        oResponse.message.errorNum = nLineNum;
    }
    if (null != sReason) {
        oResponse.message.errorReason = sReason;
    }
    if (null != nErrorCode) {
        oResponse.message.errorCode = nErrorCode;
    }
    return oResponse;
}

function judgeObjLenIsZero(jsonData) {
    return (0 == Object.keys(jsonData).length) ? true : false;
}

function getObjLen(jsonData) {
    return Object.keys(jsonData).length;
}

function getObjKeys(jsonData) {
    return Object.keys(jsonData).length;
}

function getObjVals(jsonData) {
    return Object.values(jsonData);
}

// var nowTime = new Date().myFormat("yyyy-MM-dd hh:mm:ss");
Date.prototype.myFormat = function (fmt) {
    var o = {
        "M+": this.getMonth() + 1,
        "d+": this.getDate(),
        "h+": this.getHours(),
        "m+": this.getMinutes(),
        "s+": this.getSeconds(),
        "q+": Math.floor((this.getMonth() + 3) / 3),
        "S" : this.getMilliseconds()
    };

    if (/(y+)/.test(fmt)) {
        fmt = fmt.replace(
            RegExp.$1,
            (this.getFullYear() + "").substr(4 - RegExp.$1.length)
        );
    }
    for (var k in o) {
        if (new RegExp("(" + k + ")").test(fmt)) {
            fmt = fmt.replace(
                RegExp.$1,
                (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length))
            );
        }
    }
    return fmt;
}

function judgeCb (cb) {
    return (cb && isFunction(cb));
}

function judgeParam (oBody, sParam, oReturn) {
    if (null == oReturn || undefined == oReturn) {
        return (oBody.hasOwnProperty(sParam) ? oBody[sParam] : null);
    }
    else {
        return (oBody.hasOwnProperty(sParam) ? oBody[sParam] : oReturn);
    }
}

function judgeMongoRtn(aData) {
    var aTemp = isArray(aData) ? aData : [];
    if ((0 != aTemp.length) && (isObject(aTemp[0]))) {
        return true;
    }
    else {
        return false;
    }
}

function getCurMillTime() {
    return Number(+(new Date()));
}

```