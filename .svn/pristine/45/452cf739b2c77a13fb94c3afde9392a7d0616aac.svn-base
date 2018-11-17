
//https://blog.crazyegg.com/2013/04/17/color-palettes-financial/
//http://www.dtelepathy.com/blog/inspiration/beautiful-color-palettes-for-your-next-web-project
//https://color.adobe.com/explore/newest/
//http://www.cboe.com/micro/impliedcorrelation/

// get historical synchronized http://jsfiddle.net/gh/get/jquery/1.9.1/highslide-software/highcharts.com/tree/master/samples/highcharts/demo/synchronized-charts/

//world map demo : http://jsfiddle.net/CGKNE/

var MAXTAGS = 20;
var searchCriteria;
var eventTags = $('#stock-symbol');

var isCMIDataAvailable = true;

var MAX_WIDGETS = 100;
var ticker;
var searchParams;
var tickers500Arr = [];
var tickersUserArr = [];
var rssFeedsArr = sessionStorage.rsslist;

window.DELIMITER = "_*&^%@#_";
var maxGrad = 500;
var polarityContainerPrefix = 'p_container_';
var activityContainerPrefix = 'a_container_';
var activityTrendContainerPrefix = 'a_trend_container_';
var brickWidgetPrefix = 'widget_';

var cmiData = [];
var priceData = [];
var corrData = [];
var cmiDummyData = [];
var mdDummyData = [];
var maxGrad = [];
var cmiTitle;
var mdTitle;

var refreshMD = 4;
var winSetup = null;

function isRefreshNeed() {
    if (tickersUserArr.length > 0) {
        showDiv('refreshMessage');
    }
    else {
        hideDiv('refreshMessage');
    }
}

var trendDiv, histDiv;

function GetNews(searchCrit) {
    var mArray = [];
    hideDiv('panels-front');
    hideDiv('title-front');
    //    showDiv('preloader');    
    try {
        var start1 = new Date().getTime();
        disableDiv('content');
        $.ajax({
            url: "index.ashx"
            , type: 'POST'
            , data: {
                requestType: 'GetNews'
                , searchCriteria: searchCrit
            }
            , success: function (data) {
                var mArray = data.split(DELIMITER);

                ticker = searchCrit.toUpperCase();
                var elapsed1 = new Date().getTime() - start1;

                var start2 = new Date().getTime();                

                trendDiv = activityTrendContainerPrefix + ticker.toUpperCase();
                histDiv = activityContainerPrefix + ticker.toUpperCase();
                if (mArray[0].length > 0)
                    isCMIDataAvailable = true;
                else {
                    isCMIDataAvailable = false;
                    showDiv('nomediafound');
                    setContent('preloader', '');
                    stopProgress();
                    return;
                }

                if (mArray[0].length > 0) {
                    AddWidget_1(ticker, mArray);

                    createActivityChart(ticker, mArray[0], histDiv, maxGrad);
                    createCMITrendChart(ticker, mArray[3], trendDiv, maxGrad);
                }

                if (mArray.length == 4) {
                    var percent100 = parseInt(mArray[1]) + parseInt(mArray[2]);
                    if ((mArray[1] != 0) || (mArray[2] != 0)) {
                        var pos = parseInt(mArray[1]) * 100 / percent100;
                        var neg = parseInt(mArray[2]) * 100 / percent100;

                        log('negative:' + neg + ' + ' + pos);

                        createPolarityChart(Math.round(pos), Math.round(neg), polarityContainerPrefix + ticker.toUpperCase());
                        var cmiChart = mArray[3];
                    }
                }
                var elapsed2 = new Date().getTime() - start2;
                stopProgress();
                //                appendError(ticker + ' - (db r-trip) :' + elapsed1 + ' ms.' + ', (widget render) :' + elapsed2 + ' ms.');   
                isRefreshNeed();
            }
            , error: function (data) {
                appendError('error: ' + data.data)
            }
        ,
        });
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function buildCMI_block(activityChartContainer, searchResult) {
    try {
        //        var mediaIndexIsNotAvailableMsg = '<hr/><p class="text-center">No media found for <span class="text-white">' + searchResult + '.</span></p><p class="text-center">Please change search criteria.</p>';
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
    return '<div class="activitychart bg-inverse" id="' + activityChartContainer + '"></div>';
}

function buildCMITrend_block(activityTrendChartContainer, searchResult) {
    try {
        var mediaIndexIsNotAvailableMsg = '<hr/><p class="text-center">No media found for <span class="text-white">' + searchResult + '.</span></p><p class="text-center">Please change search criteria.</p>';
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
    return '<div class="activitytrendchart bg-inverse" id="' + activityTrendChartContainer + '"></div>';
}

var i = 0;

$(".toggableIcon").click(function () {
    var mode = '';
    if ($(this).attr("data-permission-value") == "true") {
        $(this)
            .attr("data-permission-value", "false")
            .removeClass("greenIcon")
            .addClass("redIcon");
        mode = 'off';
    } else {
        $(this)
            .attr("data-permission-value", "true")
            .removeClass("redIcon")
            .addClass("greenIcon");
        mode = 'on';
    }
    log(mode);
    $('.gridly').gridly('draggable', mode);
});

function builPolarity_block(polarityChartContainer, searchResult) {
    try {
        var polarityIndexIsNotAvailableMsg = '<hr/><p>Polarity Index is not available for <span class="text-white">' + searchResult + '.</span></p><p>Please change search criteria.</p>';
        polarityIndexIsNotAvailableMsg = '';
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
    return '<div class="polaritychart bg-inverse" id="' + polarityChartContainer + '"></div>';
}


function setDraggable(mode) {
    return;
    var stat;
    if (mode == 1)
        stat = 'on';
    if (mode == 0)
        stat = 'off';
    $('.gridly').gridly('draggable', stat);
}


function toggleDragging() {
    var mode = '';
    $(this).find('i').toggleClass('fa-plus-square-o fa-2x fa-minus-square-o fa-2x');
    return;
    if ($(this).attr("data-permission-value") == "true") {
        $(this)
            .attr("data-permission-value", "false")
            .removeClass("greenIcon")
            .addClass("redIcon");
        mode = 'off';
    } else {
        $(this)
            .attr("data-permission-value", "true")
            .removeClass("redIcon")
            .addClass("greenIcon");
        mode = 'on';
    }
    log(mode);
    $('.gridly').gridly('draggable', mode);
}

function AddWidget_1(ticker, dataArray) {
    try {
        idx = ticker.toUpperCase();
        ticker = idx;
        var widgetID = brickWidgetPrefix + ticker.toUpperCase();

        var CMI_block = '';
        var CMITrend_block = '';
        var p_block = '';
        var polarityChartContainer = polarityContainerPrefix + idx;

        var activityChartContainer = activityContainerPrefix + idx;
        var activityTrendChartContainer = activityTrendContainerPrefix + idx;
        console.log(histDiv);
        CMI_block = buildCMI_block(histDiv, ticker);
        CMITrend_block = buildCMITrend_block(trendDiv, ticker);
        p_block = builPolarity_block(polarityChartContainer, ticker);

        var seriesGrad = [];

        var uiControls = '<span class="m-r-5 pull-right"><a href="javascript:drillDownNews(\'' + ticker + '\');" class="btn btn-sm btn-icon btn-circle btn-warning"><i class="fa fa-film fa-1x" title="Gets media news headlines"></a></i></span>'
                       + '<span class="m-r-5 pull-right"><a href="javascript:getHistorical(\'' + ticker + '\');" class="btn btn-sm btn-icon btn-circle btn-warning" title=""><i class="fa fa-tachometer fa-1x"></i></a></span>';

        var widget = '<div id="' + widgetID
            + '" class="brick large sm-widget sm-frame3 height-widget f-s-8 p-3">'// data-symbol="'
            + '<div><span class="sm-symbol">' + ticker + '</span>'
            + uiControls
            + '</div>'
            + CMITrend_block
            + CMI_block
            + p_block

            + '</div>';
        log('widget : ' + widget);
        event.preventDefault();
        event.stopPropagation();
        $('.gridly').append(widget);
        return $('.gridly').gridly();
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
    finally {
        setContent('preloader', '');
    }
}

$('realtimemessages').bind("DOMSubtreeModified", function () {
    log('changed');
});
function InvokeMDFeeds() {
    $("#mdListModal").modal('show');
}

function InvokeTools() {
    stopProgress();
}

function stopProgress() {
    $('.inputgroup').fadeTo(0, 1);
}

$(document).ready(function () {
    try {
        Init();

        $(".sortable").sortable({
            items: 'div:not(.unsortable)'
            , placeholder: 'sortable-placeholder'
        });
        $(".sortable").disableSelection();
        var eventTags = $('#stock-symbol');
        var addEvent = function (text) {
            //            $('#events_container').append(text + '<br>');
        };
        $('#stock-symbol').tagit({
            //  availableTags: tickers500Arr
            allowSpaces: true
            , tagLimit: MAXTAGS
            , maxTags: MAXTAGS
            , showAutocompleteOnFocus: true
            , placeholderText: 'tickers/companies/brands...'
            , caseSensitive: false
            , allowDuplicates: false
            , highlightOnExistColor: '#f71705'
            , singleFieldDelimiter: [',', ' ']
            , animate: true
            , beforeTagAdded: function (evt, ui) {
                hideDiv('nomediafound');
                setProgress();
                setContent('preloader', '<img src="images/Preloaders/31.gif"/>');
                addEvent('beforeTagAdded: ' + eventTags.tagit('tagLabel', ui.tag));
                var ticker = eventTags.tagit('tagLabel', ui.tag).removeSpecialChars();
                GetNews(ticker);                                
            }
            , afterTagAdded: function (evt, ui) {
                //				if (!ui.duringInitialization) {
                var ticker = eventTags.tagit('tagLabel', ui.tag);
                addEvent('afterTagAdded: ' + ticker);
                tickersUserArr.push(ticker);
                log('added: ' + tickersUserArr);
            }
            , beforeTagRemoved: function (evt, ui) {
                hideDiv('nomediafound');
                addEvent('beforeTagRemoved: ' + eventTags.tagit('tagLabel', ui.tag));
                RemoveWidget(eventTags.tagit('tagLabel', ui.tag));
            }
            , afterTagRemoved: function (evt, ui) {
                var ticker = eventTags.tagit('tagLabel', ui.tag).toUpperCase();
                isRefreshNeed();
            }
            , onTagClicked: function (evt, ui) {
                addEvent('onTagClicked: ' + eventTags.tagit('tagLabel', ui.tag));
            }
            , onTagExists: function (evt, ui) {
                addEvent('onTagExists: ' + eventTags.tagit('tagLabel', ui.existingTag));
            }
        });
        $(".sortable").sortable({
            items: 'div:not(.unsortable)'
            , placeholder: 'sortable-placeholder'
        });
        $(".sortable").disableSelection();
        $('#demo-form .btn').on('click', function () {
            $('#demo-form').parsley().validateFdate.now();
            validateFront();
        });
        var validateFront = function () {
            if (true === $('#demo-form').parsley().isValid()) {
                requestOfflineReport();
            }
            else { }
        };
        //    showDiv('runtimeSearchMessageContainer');
        //     setContent("runTimeSearchMessage", "...Loading analyzed regulatory documents");
        //        var dateFrom = 

        var startDate = new Date(2005, 6, 7);
        var endDate = new Date();
        //        setContent('countdays', days_between(startDate, endDate));
        animateValue("countdays", 3500, days_between(startDate, endDate), 1);
        animateValue("supportedExchanges", 1, 38, 3000);
        animateValue("newsandagencies", 1, 746, 3000);
        //        setContent('supportedExchanges', '17');
        //        setContent('newsandagencies', '24');
        isChartSettingsHidden = false;
        isSettingsHidden = false;
        isDocSourceHidden = false;
        isMDHidden = false;
        isMDrequired = false;
        var leftOffset = 180;
        var topOffset = 200;
        var rsssource = '';
        isRefreshNeed();
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
});

function clearAllWidgets() {
    try {
        $('#stock-symbol').tagit('removeAll');
        tickersUserArr.length = 0;
        isRefreshNeed();
        setContent('gridly', '');

        $('.gridly .brick').each(function () {
        });

    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }

}

function myIP() {
    if (window.XMLHttpRequest) xmlhttp = new XMLHttpRequest();
    else xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
    xmlhttp.open("GET", "http://api.hostip.info/get_html.php", false);
    xmlhttp.send();
    hostipInfo = xmlhttp.responseText.split("\n");
    for (i = 0; hostipInfo.length >= i; i++) {
        ipAddress = hostipInfo[i].split(":");
        if (ipAddress[0] == "IP") return ipAddress[1];
    }
    return false;
}

function InfoGeneral(title, message) {
    try {
        BootstrapDialog.show({
            cssClass: "bootstraplayout"
            , size: BootstrapDialog.SIZE_NORMAL
            , type: BootstrapDialog.TYPE_INFO
            , title: '<i class="fa fa-info-circle fa-inverse pull-left"></i><b>' + title + '</b>'
            , message: '<div class="infomessagelayout">' + message + '</div>'
            , buttons: [{
                icon: 'glyphicon glyphicon-check'
                , cssClass: 'btn-primary'
                , label: 'Close'
                , action: function (dialogItself) {
                    dialogItself.close();
                }
            }]
        });
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function afterTagAdded(ticker) { //event to gat input
    var maxWidgetsPerRow = 6;
}

function posNegClick() {
}

function getClickedTicker() {
    var elem = document.getElementById("ticker-holder");
    ticker = elem.getAttribute('ticker-now');
}

function drillDownNews(tick) {
    var url = 'news.html';
    //    getClickedTicker();
    localStorage.setItem('news', tick);
    winSetup = window.open(url, '_blank');
    winSetup.focus();

}

function InvokePrivacyPolicy() {
    try {
        BootstrapDialog.show({
            cssClass: "bootstraplayout",
            size: BootstrapDialog.SIZE_NORMAL,
            type: BootstrapDialog.TYPE_INFO,
            title: '<i class="fa fa-info-circle fa-inverse pull-left fa-1x"></i><b>Privacy Policy</b>',
            message: '<div><object data="documents/Privacy-policy-3677674.pdf" type="application/pdf" id="pdfobject"></object></div>',
            cssClass: 'privacypolicy-dialog',
            buttons: [{
                icon: 'glyphicon glyphicon-check',
                cssClass: 'btn-primary btn-close',
                label: 'Close',
                action: function (dialogItself) {
                    dialogItself.close();
                }
            }]
        });
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function InvokeContacts() {
    window.location.href = 'mailto:info.narasa.com';
}

function animateValue(id, start, end, duration) {
    var range = end - start;
    var current = start;
    var increment = end > start ? 1 : -1;
    var stepTime = Math.abs(Math.floor(duration / range));
    var obj = document.getElementById(id);
    var timer = setInterval(function () {
        current += increment;
        obj.innerHTML = current;
        if (current == end) {
            clearInterval(timer);
        }
    }, stepTime);
}

function changeHtml(selector, html) {
    var elem = $(selector);
    jQuery.event.trigger('htmlchanging', { elements: elem, content: { current: elem.html(), pending: html } });
    elem.html(html);
    jQuery.event.trigger('htmlchanged', { elements: elem, content: html });
}

function printMessage(message) {
    setContent('realtimemessages', message);
}

function clearMessage() {
    setContent('realtimemessages', '');
}

function ConvertToCSV(objArray) {
    try {
        var array = typeof objArray != 'object' ? JSON.parse(objArray) : objArray;
        var str = '';
        for (var i = 0; i < array.length; i++) {
            var line = '';
            str += array[i].text + ',';
        }
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);

    }
    return str.replace(/,\s*$/, "");
}

function logErrors(method, errorMessage) {
    console.error(method + ' ---> Error:[' + errorMessage + ']');
    //  document.getElementById("error").innerHTML = '[' + method + ']: ' + errorMessage;
}


function refreshWidgets() {
    timer1.stop();
}

var timer1;

function Init() {
    try {
        setContent('preloader', '');
        hideDiv('nomediafound');
        $('.gridly').gridly({
            base: 60, // px 
            gutter: 3, // px
            columns: 20
        });
        //  $('[data-toggle="tooltip"]').tooltip();
        $('[data-toggle="popover"]').popover();

        appendError('init: refresh time:' + refreshMD);
        $(".toggableIcon").each(function () {
            if ($(this).attr("data-permission-value") == "true") {
                $(this).addClass("greenIcon");
            } else {
                $(this).addClass("redIcon");
            }
        });

        var display1 = document.querySelector('#time1');
        timer1 = new CountDownTimer(refreshMD);

        timer1.onTick(format(display1)).onTick(restart).start();

        function restart() {
            if (this.expired()) {
                setTimeout(function () {
                    timer1.start();
                    //   refreshWidgets();
                },
                1000);
            }
        }

        function format(display) {
            return function (minutes, seconds) {
                minutes = minutes < 10 ? "0" + minutes : minutes;
                seconds = seconds < 10 ? "0" + seconds : seconds;
                display.textContent = minutes + ':' + seconds;
            };
        }


        //ii ge

        //$.ajax({
        //    url: "index.ashx"
        //    , type: 'POST'
        //    , data: {
        //        requestType: 'GetTickers'
        //        , searchCriteria: ""
        //    }
        //    , success: function (data) {
        //        var dataAll = data.split(DELIMITER);
        //        rssFeedsArr = dataAll[0].replace(/(?:\r\n|\r|\n)/g, '<br />');
        //        sessionStorage.rsslist = rssFeedsArr;

        //        //                log(sessionStorage.rsslist);

        //        var tarr = dataAll[1].split('\n');
        //        for (var i = 0; i < tarr.length; i++) {
        //            tickers500Arr.push(tarr[i].toUpperCase());
        //        }
        //    }
        //    , error: function (data) {
        //        log('error: ' + data.data)
        //    }
        //,
        //});

        setInterval(function () {
            var currentdate = new Date();
            var datetime = "Last synch to MD provider: "
                            + currentdate.getHours() + ":"
                            + currentdate.getMinutes() + ":"
                            + currentdate.getSeconds();
            //       logError(datetime);
            //ii            smBuildWidgets(); // it invokes MD component for refreshing
        }, refreshMD * 1000);
    }

    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

window.onfocus = function () {

};

function InvokeCMIFeeds() {
    try {
        $("#CMIFeeds").modal('show');
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function GetNCharacters(str, number) {
    try {
        var ret = '';
        if (str.length > 0) {
            ret = str.substring(0, number) + ' ...';
        }
        if (str.length < number) {
            ret = str.substring(0, number);
        }
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);

    }

    return ret;
}

function InvokeInfoRSS() {
    try {
        $("#rssListModal").modal('show');
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function isTicker(tag) {
    try {
        return tickers500Arr.indexOf(tag) > -1;
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function oddOrEven(x) {
    return (x & 1) ? "odd" : "even";
}

$('.closeall').click(function () {
    $('.panel-collapse.in').collapse('hide');
});

$('.openall').click(function () {
    $('.panel-collapse:not(".in")').collapse('show');
});

var colWidth = 310 / 2 - 25;

function RefreshBricks() {
    try {
        log('tickers:' + tickersUserArr);
        log(tickersUserArr);

        setContent('gridly', '');
        log(tickersUserArr);

        for (var i = 0; i < tickersUserArr.length; i++) {
            GetNews(tickersUserArr[i]);
        }
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function RemoveWidget(ticker) {
    try {
        log('ticker :' + ticker);
        var widgetID = brickWidgetPrefix + ticker.toUpperCase();
        log('to be removed:' + widgetID);
        var index = tickersUserArr.indexOf(ticker);
        if (index > -1) {
            tickersUserArr.splice(index, 1);
        }
        log(tickersUserArr);
        document.getElementById(widgetID).remove();
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }

}

function randomBetween(min, max) {
    if (min < 0) {
        return min + Math.random() * (Math.abs(min) + max);
    } else {
        return min + Math.random() * max;
    }
}

function buildHeader_block(MDName, ticker) {
    try {
        if (!MDName) {
            MDName = '';
        }
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }

    return '<div class="sm-symbol pull-left m-r-10">' + GetNCharacters(ticker, 10) + '</div>' + '<div id="md"><div class="sm-company">' + MDName + '</div>';
}

function disableDiv(id) {
    document.getElementById(id).disabled = true;
    log(id);
}

function enableDiv(id) {
    document.getElementById(id).disabled = false;
}

function InvokeAdmin() {
    alert("Unable to Authenticate. Custom Business Connectors are not avaialable.");
}

function InvokeSetup() {
    var url = 'setup.html';
    localStorage.setItem('news', ticker);
    winSetup = window.open(url, '_blank');
    winSetup.focus();
}

// negative, positive, container name
function createPolarityChart(ny, py, container) {    
    try {
        $(function () {
            var colorPos = '#2C6700';
            var colorNeg = '#B22222';
            var colorTitle = '#FFCF79';
            var chart;
            chart = new Highcharts.Chart({
                chart: {
                    backgroundColor: '#424242'
                    , renderTo: container
                    , events: {
                        load: function (event) {
                            //  hideDiv('preloader');
                            setContent('preloader', '');
                        }
                    }
                    , type: 'bar'
                    , position: 'horizontal'
                    , margin: [0, 0, 0, 0]
                }
                , title: {
                    text: '(' + ny + '%) Media Polarity Index (' + py + '%)'
                    , style: {
                        color: colorTitle
                        , font: '11px "Trebuchet MS", Verdana, sans-serif'
                    }
                }
                , legend: {
                    enabled: false
                }
                ,
                xAxis: {
                    lineWidth: 0
                    , labels: {
                        enabled: false
                    }
                    , minorTickLength: 0
                    , tickLength: 0
                }
                , credits: {
                    enabled: false
                }
                , yAxis: {
                    min: 0
                    , title: {
                        text: ''
                    }
                    , lineWidth: 0
                    , gridLineWidth: 0
                    , lineColor: 'transparent'
                    , showInLegend: false
                    , labels: {
                        enabled: false
                    }
                    , minorTickLength: 0
                    , tickLength: 0
                }
                ,
                plotOptions: {
                    bar: {
                        stacking: 'percent'
                    }
                    , series: {
                        pointPadding: 0
                        , groupPadding: 0
                    }
                }
                , tooltip: {
                    enabled: false
                }
                , series: [{
                    name: 'positive'
                        , color: colorPos
                        , data: [py]
                        , dataLabels: {
                            enabled: false
                            , style: {
                                fontSize: 13
                                , fontWeight: 'bold'
                                , textShadow: false

                            }
                            , color: '#000099'
                            , valuePostfix: '%'
                        }
                }
                    , {
                        name: 'negative'
                        , color: colorNeg
                        , data: [ny]
                        , dataLabels: {
                            enabled: false
                            , style: {
                                fontSize: 16
                                , fontWeight: 'bold'
                                , textShadow: false
                            }
                            , color: 'yellow'
                            , valuePostfix: '%'
                        }
                    }]
                , exporting: {
                    enabled: false
                    , buttons: [
                        {
                            symbol: 'square'
                            , x: 0
                            , symbolFill: '#B5C9DF'
                            , hoverSymbolFill: '#779ABF'
                            , _titleKey: 'printButtonTitle'
                            , onclick: function () {
                                posNegClick()
                            }
                        }
                    ]
                }
            });
        });
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function createCMITrendChart(ticker, stringTimeLine, container, maxGrad, MIN, MAX) {
    try {
        var dataArr = JSON.parse('[' + stringTimeLine + ']');
        var MAX = dataArr.max();
        var MIN = dataArr.min();
        var fillColor = '#0bfff';
        var cmiScore = -1962;

        if (dataArr[0] < dataArr[dataArr.length - 1]) {
            fillColor = 'rgba(0, 255, 0, 0.4)';
            cmiScore = dataArr[dataArr.length - 1];
        }
        if (dataArr[0] > dataArr[dataArr.length - 1]) {
            fillColor = 'rgba(255, 0, 0, 0.4)';
            cmiScore = dataArr[0];
        }
        //http://jsfiddle.net/gh/get/library/pure/highcharts/highcharts/tree/master/samples/highcharts/plotoptions/area-fillopacity/
        var chart;
        $(function () {
            chart = new
            Highcharts.Chart({
                chart: {
                    margin: [0, 0, 0, 0]
                                , backgroundColor: {
                                    linearGradient: [0, 0, 0, 244]
                        , stops: [
                            [0, 'black']
                          , [1, 'grey']
                          , [2, 'white']
                        ]
                                }
                    , type: 'area'
                    , renderTo: container
                    , events: {

                        load: function (event) {
                            setContent('preloader', '');
                        }
                        //, click: function (event) {
                        //    $('.gridly').gridly('draggable', 'off');
                        //}
                    }
                ,
                }
                 , title: {
                     text: 'Combined Media Index Momentum'
                    , align: 'left'
                    , style: {
                        color: 'yellow'
                        , font: '10px "Trebuchet MS", Verdana, sans-serif'
                    }
                 }
                , subtitle: {
                    text: '(Activity of Discussion)'
                    , align: 'left'
                    , style: {
                        color: 'yellow'
                        , font: '10px "Trebuchet MS", Verdana, sans-serif'
                    }
                }
                , legend: {
                    enabled: false
                }
                , scrollbar: {
                    enabled: false
                }
                , credits: {
                    enabled: false
                }
                ,
                scrollbar: {
                    enabled: false
                }
                , yAxis: {
                    type: 'logarithmic'
                    , lineWidth: 0
                    , gridLineWidth: 1
                     , gridLineColor: '#1B2631'
                    , lineColor: 'transparent'
                    , showInLegend: false
                    , labels: {
                        enabled: false
                    }
                    , minorTickLength: 0
                    , tickLength: 0
                }
                , tooltip: {
                    enabled: false

                }
                , plotOptions: {
                    series: {
                        fillOpacity: 0.3
                        , pointPadding: 0
                        , groupPadding: 0
                    }
                    , area: {
                        pointStart: 0
                        , marker: {
                            enabled: true
                            , symbol: 'circle'
                            , radius: 1
                            , states: {
                                hover: {
                                    enabled: false
                                }
                            }
                        }
                    }
                }
                , series: [{
                    showLegend: false
                    , style: {
                        fontSize: 16
                        , fontWeight: 'bold'
                        , textShadow: false
                    }
                    , color: '#339966'
                    , name: 'Combined Media Index Momentum'
                    , align: 'left'
                    , fillColor: fillColor
                    , data: dataArr

                }, ]
                , exporting: {
                    enabled: false
                    , buttons: {
                        contextButton: {
                            enabled: false
                        },
                        customButton: {
                            x: -2,
                            onclick: function () {
                            }
                            , symbol: 'circle'
                        },
                    }
                }
            },


                function (chart) { // on complete

                    chart.renderer.text('<span style="color: cyan">' + cmiScore + '</span>', 90, 60)
                        .css({
                            color: '#4572A7',
                            fontSize: '20px',
                            fontWeight: 'bold'
                        })
                        .add();

                }



                );
        });
        //     chart.xAxis[0].setExtremes(event.xAxis[0].min,  event.xAxis[0].max);
    }
    catch (err) {
        alert(err);
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function createActivityChart(ticker, stringTimeLine, container, maxGrad, MIN, MAX) {
    try {
        var dataArr = JSON.parse('[' + stringTimeLine + ']');
        var MAX = dataArr.max();
        var MIN = dataArr.min();
        var chart;
        $(function () {
            chart = new
            Highcharts.Chart({
                chart: {
                    margin: [0, 0, 0, 0]
                    //                    , type: 'areaspline'
                    , type: 'areaspline'
                    , renderTo: container
                        , zoomType: 'x'
                    , resetZoomButton: {
                        theme: {
                            fill: 'orange',
                            stroke: 'navy',
                            r: 3,
                            states: {
                                hover: {
                                    fill: '#21618C',
                                    style: {
                                        color: 'white'
                                    }
                                }
                            }
                        }
                        , position: {
                            align: 'right', // right by default
                            verticalAlign: 'top',
                            x: 0,
                            y: 0
                        },
                        relativeTo: 'chart'
                    }
                    , events: {
                        load: function (event) {
                            //  hideDiv('preloader');
                            setContent('preloader', '');
                        }
                        //, click: function (event) {
                        //    $('.gridly').gridly('draggable', 'off');
                        //}
                    }
                    , backgroundColor: {
                        linearGradient: [0, 0, 0, 244]
                        , stops: [
                            [0, 'black']
                          , [1, 'grey']
                          , [2, 'white']
                        ]
                    }
                ,
                }
                , title: {
                    text: 'Combined Media Index History'
                    , align: 'left'
                    , style: {
                        color: 'cyan',
                        font: 'bold 10px "Trebuchet MS", Verdana, sans-serif'
                    }
                }
                , legend: {
                    enabled: false
                }
                , scrollbar: {
                    enabled: false
                }
                , xAxis: {
                    //                    allowDecimals: false,
                    type: "datetime",
                    tickInterval: 10
                    , gridLineWidth: 1
                    , gridLineColor: '#1B2631'
                    , dateTimeLabelFormats: {
                        //  day: '%H'
                        day: '%b %e'
                    }
                     , lineWidth: 0
                    , labels: {
                        enabled: false
                    }
                    , minorTickLength: 0
                    , tickLength: 0
                    //     , tickeInterval: 3600 * 1000
                }
                , credits: {
                    enabled: false
                }
                ,

                rangeSelector: {
                    enabled: false
                },
                navigator: {
                    enabled: false,
                    margin: 17
                },
                scrollbar: {
                    enabled: true,
                    height: 11,
                    barBackgroundColor: '#21618C',
                    barBorderRadius: 7,
                    barBorderWidth: 0,
                    buttonBackgroundColor: 'gray',
                    buttonBorderWidth: 0,
                    buttonArrowColor: 'yellow',
                    buttonBorderRadius: 7,
                    rifleColor: 'orange',
                    trackBackgroundColor: 'orange',
                    trackBorderWidth: 1,
                    trackBorderColor: 'silver',
                    trackBorderRadius: 7
                }
                , yAxis: {
                    gridLineWidth: 1
                 , gridLineColor: '#1B2631'
                   , type: 'logarithmic',
                    lineWidth: 1
   , gridLineWidth: 1
   , gridLineColor: '#1B2631'
                    , title: {
                        text: ''
                    }
                    , lineWidth: 1
                    , gridLineWidth: 1
                    , gridLineColor: '#1B2631'
                    , lineColor: '#green'
                    , showInLegend: false
                    , labels: {
                        enabled: false
                    }
                    , minorTickLength: 0
                    , tickLength: 0
                }
                , tooltip: {
                    xDateFormat: '%Y/%m/%d',
                    backgroundColor: '#FCFFC5',
                    style: {
                        fontSize: 8
                , fontWeight: 'bold'
                , textShadow: false
                    },
                    pointFormat: '{series.name}: <b>{point.y:,.0f}</b>'
                    ,
                    positioner: function () {
                        return { x: 10, y: 70 };
                    }
                }
                , plotOptions: {
                    series: {
                        pointPadding: 0
                        , groupPadding: 0
                    }
                    , area: {
                        pointStart: 0
                        , marker: {
                            enabled: true
                            , symbol: 'circle'
                            , radius: 1
                            , states: {
                                hover: {
                                    enabled: true
                                }
                            }
                        }
                    }
                }
                , series: [{
                    showLegend: false
                    , style: {
                        fontSize: 10
                        , fontWeight: 'bold'
                        , textShadow: false
                    }
                    //http://blog.visme.co/color-combinations/
                    , color: '#16B5D6'
                    , name: 'Combined Media Index'
                    , fillColor:
                        {
                            linearGradient: [0, 0, 0, 300]
                        , stops: [
                         [0, Highcharts.getOptions().colors[0]],
                        [1, 'rgba(2,2,0,1)']
                        ]
                        }
                    , data: dataArr
                    , pointInterval: 24 * 3600 * 1000
                }, ]
                , exporting: {
                    enabled: false
                    , buttons: {
                        contextButton: {
                            enabled: false
                        },
                        customButton: {
                            x: -2,
                            onclick: function () {
                            }
                            , symbol: 'circle'
                        },
                    }
                }
            });
        });
        //     chart.xAxis[0].setExtremes(event.xAxis[0].min,  event.xAxis[0].max);
    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}

function getHistorical(crit) {
    try {
        var start1 = new Date().getTime();
        //        getClickedTicker();
        localStorage.setItem('news', crit);

        var url = 'historical.html';
        var success = false;
        cmiTitle = 'Combined Media Index';
        mdTitle = 'Market Data - Daily Last Price';
        corrTitle = 'Market Data - Daily Last Price Correlation';
        maxGrad = 500;
        var url = 'historical.html';
        var wh = window.open(url, '_blank');

        disableDiv('content');

        var request = $.ajax({
            url: "index.ashx"
             , type: 'POST'
            , async: false
            , data: {
                requestType: 'GetHistorical'
                , searchCriteria: crit
            },
            beforeSend: function (xhr, opts) {
                log('before send ' + opts);
            }
        }).done(function (data) {

            log(data);

            cmiData = JSON.parse('[' + data.split(window.DELIMITER)[0] + ']');
            cmiDummyData = JSON.parse('[' + data.split(window.DELIMITER)[1] + ']');
            priceData = JSON.parse('[' + data.split(window.DELIMITER)[2] + ']');
            corrData = JSON.parse('[' + data.split(window.DELIMITER)[3] + ']');

            log('cmi:' + cmiData);
            log('price:' + priceData);
            log('corr data:' + corrData);
            log('cmiDummyData: ' + cmiDummyData + '   mdDummyData: ' + mdDummyData);
            log('cmiData:' + cmiData);
            log('length' + cmiData.length);

            log('priceData:' + priceData);
            success = true;

        }).fail(function (data) {
            log('fail');

        }).always(function (data) {
            log('always');

        });
        if (success) {
            wh.focus();
            stopProgress();
        }

    }
    catch (err) {
        logErrors(arguments.callee.name.toString(), err.message);
    }
}


