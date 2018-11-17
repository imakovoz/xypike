window.rsslist = 'rr';

var DEBUG = true;

//hideDiv('message-block');

function log(msg, obj) {
    if (DEBUG) {
        if (typeof obj !== 'undefined') {
            console.log(msg, obj);
        }
        else {
            console.log(msg);
        }
    }
}


function delay(ms) {
    ms += new Date().getTime();
    while (new Date() < ms) { }
}


function logError(msg) {
    if (DEBUG) {
        setContent('message-block', msg);
    }
}

function appendError(msg) {
    if (DEBUG) {
        appendContent('message-block', "<BR/>" + msg + "<BR/");
    }
}

Array.prototype.average = function () {
    var sum = 0;
    var j = 0;
    for (var i = 0; i < this.length; i++) {
        if (isFinite(this[i])) {
            sum = sum + parseFloat(this[i]);
            j++;
        }
    }
    if (j === 0) {
        return 0;
    } else {
        return sum / j;
    }
}

Array.prototype.clone = function () {
    return this.slice(0);
}

Array.prototype.max = function () {
    return Math.max.apply(null, this);
};

Array.prototype.min = function () {
    return Math.min.apply(null, this);
};

Date.prototype.yyyymmdd = function () {
    var yyyy = this.getFullYear().toString();
    var mm = (this.getMonth() + 1).toString(); // getMonth() is zero-based
    var dd = this.getDate().toString();
    return yyyy + "/" + (mm[1] ? mm : "0" + mm[0]) + "/" + (dd[1] ? dd : "0" + dd[0]); // padding
};

String.prototype.removeSpecialChars = function () {
    var res = '';
    res = this.replace(/[^A-Za-z.]/g, "");
    return (res)
}

function days_between(startDate, endDate) {

    return (Date.UTC(endDate.getYear(), endDate.getMonth(), endDate.getDate()) -
                   Date.UTC(startDate.getYear(), startDate.getMonth(), startDate.getDate())) / 86400000

}

function hideDiv(id) {
    document.getElementById(id).style.display = 'none';
}

function showDiv(id) {
    document.getElementById(id).style.display = 'block';
}

function setContent(id, content) {
    document.getElementById(id).innerHTML = content;
}

function setValue(id, content) {    
    document.getElementById(id).value = content;
}

function appendContent(id, content) {
    $('#' + id).append(content);
}

function prependContent(id, content) {
    $('#' + id).prepend(content);
}

function cleanContent(id) {
    $('#' + id).html('');
}

function getContent(id) {
    if ($(id).length > 0) {
        return document.getElementById(id).value;
    }
}

function changeColorOfElement(id, color) {
    document.getElementById(id).style.color = color;
}

function changeClassOfElement(id, className) {
    document.getElementById(id).className = className;
}

function addClassToElement(id, className) {
    return;
    var element = document.getElementById(id);
    element.classList.add(className);
}

function removeClassFromElement(id, className) {
    var element = document.getElementById(id);
    element.classList.remove(className);
}

function getElementsByClassName(node, classname) {
    if (node.getElementsByClassName) { // use native implementation if available
        return node.getElementsByClassName(classname);
    }
    else {
        return (function getElementsByClass(searchClass, node) {
            if (node == null) node = document;
            var classElements = []
                , els = node.getElementsByTagName("*")
                , elsLen = els.length
                , pattern = new RegExp("(^|\\s)" + searchClass + "(\\s|$)")
                , i, j;
            for (i = 0, j = 0; i < elsLen; i++) {
                if (pattern.test(els[i].className)) {
                    classElements[j] = els[i];
                    j++;
                }
            }
            return classElements;
        })(classname, node);
    }
}

function SetAtrribute(className, attributeName, addedAttribute) {
    try {
        var divs = document.getElementsByClassName(className);
        for (var i = 0; i < divs.length; i++) {
            divs[i].setAttribute(attributeName, addedAttribute);
            // Another option: divs[i].myAttr = "added attribute";
        }

    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }

}

function setAttributeById(id, attribute, attributeValue) {
    document.getElementById(id).setAttribute(attribute, attributeValue);
}
window.tagRemoved = function (data) { }

function setProgress() {
    $('.inputgroup').fadeTo(0, 0.1);
    disableDiv('inputgroup');
}

function CountDownTimer(duration, granularity) {
    this.duration = duration;
    this.granularity = granularity || 1000;
    this.tickFtns = [];
    this.running = false;
}

CountDownTimer.prototype.start = function () {
    if (this.running) {
        return;
    }
    this.running = true;
    var start = Date.now(),
        that = this,
        diff, obj;

    (function timer() {
        diff = that.duration - (((Date.now() - start) / 1000) | 0);

        if (diff > 0) {
            setTimeout(timer, that.granularity);
        } else {
            diff = 0;
            that.running = false;
        }

        obj = CountDownTimer.parse(diff);
        that.tickFtns.forEach(function (ftn) {
            ftn.call(this, obj.minutes, obj.seconds);
        }, that);
    }());
};

CountDownTimer.prototype.onTick = function (ftn) {
    if (typeof ftn === 'function') {
        this.tickFtns.push(ftn);
    }
    return this;
};

CountDownTimer.prototype.expired = function () {
    return !this.running;
};

CountDownTimer.parse = function (seconds) {
    return {
        'minutes': (seconds / 60) | 0,
        'seconds': (seconds % 60) | 0
    };
};


function getFormattedDate() {
    var date = new Date();

    var month = date.getMonth() + 1;
    var day = date.getDate();
    var hour = date.getHours();
    var min = date.getMinutes();
    var sec = date.getSeconds();

    month = (month < 10 ? "0" : "") + month;
    day = (day < 10 ? "0" : "") + day;
    hour = (hour < 10 ? "0" : "") + hour;
    min = (min < 10 ? "0" : "") + min;
    sec = (sec < 10 ? "0" : "") + sec;

    var str = date.getFullYear() + "-" + month + "-" + day + "_" + hour + ":" + min + ":" + sec;

    /*alert(str);*/

    return str;
}


/****************************************************************************

myDateObject.customFormat(formatString)
-------------------------------------------------
arguments:
	myDateObject - a JavaScript Date object
	formatString - a string containing one or more tokens to be replaced
	

returns:
	a new string based on formatString, with all valid tokens replaced with
	values from dateObject
	

description:
	Use customFormat(...) to format a Date object in anyway you like.
	Any token (from the list below) gets replaced with the corresponding value.
	

	token:     description:             example:
	#YYYY#     4-digit year             1999
	#YY#       2-digit year             99
	#MMMM#     full month name          February
	#MMM#      3-letter month name      Feb
	#MM#       2-digit month number     02
	#M#        month number             2
	#DDDD#     full weekday name        Wednesday
	#DDD#      3-letter weekday name    Wed
	#DD#       2-digit day number       09
	#D#        day number               9
	#th#       day ordinal suffix       nd
	#hhhh#     2 digit military hour    17
	#hhh#      military/24-based hour   17
	#hh#       2-digit hour             05
	#h#        hour                     5
	#mm#       2-digit minute           07
	#m#        minute                   7
	#ss#       2-digit second           09
	#s#        second                   9
	#ampm#     "am" or "pm"             pm
	#AMPM#     "AM" or "PM"             PM
	

notes:
	If you want the current date and time, use "new Date()".
	e.g.
	var curDate = new Date();
	curDate.customFormat("#DD#/#MM#/#YYYY#");
	

	If you want all-lowercase or all-uppercase versions of the weekday/month,
	use the toLowerCase() or toUpperCase() methods of the resulting string.
	e.g.
	curDate.customFormat("#DDDD#, #MMMM# #D#, #YYYY#").toLowerCase();

	If you use the same format in many places in your code, I suggest creating
	a wrapper function to this one, e.g.:
	Date.prototype.myDate=function(){
		return this.customFormat("#D#-#MMM#-#YYYY#").toLowerCase();
	}
	Date.prototype.myTime=function(){
		return this.customFormat("#h#:#mm##ampm#");
	}

	var now = new Date();
	alert(now.myDate());

	
examples:
	var now = new Date();
	var myString = now.customFormat("#YYYY#-#MM#-#DD#"); 
	alert(myString);
	-->1999-11-19
	
	var myString = now.customFormat("#DDD#, #MMM#-#D#-#YY#"); 
	alert(myString);
	-->Fri, Nov-19-99
	
	var myString = now.customFormat("#h#:#mm# #ampm#"); 
	alert(myString);
	-->4:34 pm

	var myString = now.customFormat("#hhh#:#mm#:#ss#"); 
	alert(myString);
	-->16:34:26

	var myString = now.customFormat("#DDDD#, #MMMM# #D##th#, #YYYY# @ #h#:#mm# #ampm#"); 
	alert(myString);
	-->Friday, November 19th, 1999 @ 4:34 pm
	
****************************************************************************/

Date.prototype.customFormat = function (formatString) {
    var YYYY, YY, MMMM, MMM, MM, M, DDDD, DDD, DD, D, hhhh, hhh, hh, h, mm, m, ss, s, ampm, AMPM, dMod, th;
    var dateObject = this;
    YY = ((YYYY = dateObject.getFullYear()) + "").slice(-2);
    MM = (M = dateObject.getMonth() + 1) < 10 ? ('0' + M) : M;
    MMM = (MMMM = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"][M - 1]).substring(0, 3);
    DD = (D = dateObject.getDate()) < 10 ? ('0' + D) : D;
    DDD = (DDDD = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"][dateObject.getDay()]).substring(0, 3);
    th = (D >= 10 && D <= 20) ? 'th' : ((dMod = D % 10) == 1) ? 'st' : (dMod == 2) ? 'nd' : (dMod == 3) ? 'rd' : 'th';
    formatString = formatString.replace("#YYYY#", YYYY).replace("#YY#", YY).replace("#MMMM#", MMMM).replace("#MMM#", MMM).replace("#MM#", MM).replace("#M#", M).replace("#DDDD#", DDDD).replace("#DDD#", DDD).replace("#DD#", DD).replace("#D#", D).replace("#th#", th);

    h = (hhh = dateObject.getHours());
    if (h == 0) h = 24;
    if (h > 12) h -= 12;
    hh = h < 10 ? ('0' + h) : h;
    hhhh = hhh < 10 ? ('0' + hhh) : hhh;
    AMPM = (ampm = hhh < 12 ? 'am' : 'pm').toUpperCase();
    mm = (m = dateObject.getMinutes()) < 10 ? ('0' + m) : m;
    ss = (s = dateObject.getSeconds()) < 10 ? ('0' + s) : s;
    return formatString.replace("#hhhh#", hhhh).replace("#hhh#", hhh).replace("#hh#", hh).replace("#h#", h).replace("#mm#", mm).replace("#m#", m).replace("#ss#", ss).replace("#s#", s).replace("#ampm#", ampm).replace("#AMPM#", AMPM);
}


function toggleFullScreen(elem) {
    // ## The below if statement seems to work better ## if ((document.fullScreenElement && document.fullScreenElement !== null) || (document.msfullscreenElement && document.msfullscreenElement !== null) || (!document.mozFullScreen && !document.webkitIsFullScreen)) {
    if ((document.fullScreenElement !== undefined && document.fullScreenElement === null) || (document.msFullscreenElement !== undefined && document.msFullscreenElement === null) || (document.mozFullScreen !== undefined && !document.mozFullScreen) || (document.webkitIsFullScreen !== undefined && !document.webkitIsFullScreen)) {
        if (elem.requestFullScreen) {
            elem.requestFullScreen();
        } else if (elem.mozRequestFullScreen) {
            elem.mozRequestFullScreen();
        } else if (elem.webkitRequestFullScreen) {
            elem.webkitRequestFullScreen(Element.ALLOW_KEYBOARD_INPUT);
        } else if (elem.msRequestFullscreen) {
            elem.msRequestFullscreen();
        }
 
    } else {
        if (document.cancelFullScreen) {
            document.cancelFullScreen();
        } else if (document.mozCancelFullScreen) {
            document.mozCancelFullScreen();
        } else if (document.webkitCancelFullScreen) {
            document.webkitCancelFullScreen();
        } else if (document.msExitFullscreen) {
            document.msExitFullscreen();
        }
  
    }
}

 