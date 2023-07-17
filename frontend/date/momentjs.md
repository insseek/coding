# Moment.js的常用方法

组件官网：Moment.js 中文网

```javascript
// 上周一
var lastWeekMonday = moment().week(moment().week() - 1).startOf("week");
// 本周一
var thisWeekMonday = moment().startOf("week");
//本周日
var ThisWeekSunday = moment().endOf("week");
//下周日
var NextWeekSunday = moment().week(moment().week() + 1).endOf("week");
 
// 上一个月
let lastYearMonth = moment(date).subtract(1, "month").format("YYYY-MM");
// 下一个月
let nextYearMonth = moment(date).subtract(-1, "month").format("YYYY-MM");
 
 
// 上月天数
let lastDayNums = moment(date).subtract(1, 'month').daysInMonth();
// 当月天数
let activeDayNUms = moment(date).daysInMonth();
// 下月天数
let nextDayNums = moment(date).subtract(-1, 'month').daysInMonth();
 
// 上月最后一日星期几
let lastMonthLastDayWeekday = moment(lastYearMonth).date(lastDayNums).weekday();
// 当月1日为星期几
let activeMonthFirstDayWeekday = moment(date).date(1).weekday();
// 当月最后一日星期几
let activeMonthLastDayWeekday = moment(date).date(activeDayNUms).weekday();
// 下月1日为星期几
let nextMonthFirstDayWeekday = moment(nextYearMonth).date(1).weekday();
 
 
 
// 根据年月获取该月的日历数组 参数 YYYY-MM   getEveryMonthDays('2019-4')
function getEveryMonthDays(currentMonthStr) {
    // 当月第一天
    let thisMonthFirstDay = moment(currentMonthStr).startOf("month");
    // 当月最后一天
    let thisMonthEndDay = moment(currentMonthStr).endOf("month");
    // 当月第一天所在周的周一
    let startDay = thisMonthFirstDay.startOf("week");
    let startDayStr = startDay.format('YYYY-MM-DD');
    //当月最后一天所在周的周日
    let endDay = thisMonthEndDay.endOf("week");
    let endDayStr = endDay.format('YYYY-MM-DD');
    //当月最后一天所在周的周日 与 当月第一天所在周的周一的相隔天数
    let dayNums = endDay.diff(startDay, 'days');
 
    let dayGroups = [];
    for (let i = 0; i < (dayNums + 1) / 7; ++i) {
        var startIndex = i * 7;
        var endIndex = (i + 1) * 7;
        var itemList = [];
        for (let j = startIndex; j < endIndex; j++) {
            let currentDate = moment(startDayStr).add(j, 'days');
            let dateData = {
                day: currentDate.format('YYYY-MM-DD'),
                weekday: currentDate.weekday(),
                num: currentDate.format('D'),
                isCurrentMonth: currentDate.format('YYYY-MM') == moment(currentMonthStr).format('YYYY-MM')
            }
            itemList.push(dateData)
        }
        dayGroups.push(itemList);
    }
    return dayGroups;
}
```



