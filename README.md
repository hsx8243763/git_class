# git_class
我的第一个git小项目

 <title>Title</title>
  
  
  <style》
        #calenda {
            position: absolute;
            padding: 5px;
            border: 1px solid #000000;
            background: #6277ce;
            display: none;
        }
        #todayTime {
            padding: 5px 0;
            font-size: 40px;
            color: white;
        }
        #todayDate {
            padding: 5px 0;
            font-size: 24px;
            color: #ffcf88;
        }
        #tools {
            padding: 5px 0;
            height: 30px;
            color: white;
        }
        #tools .l {
            float: left;
        }
        #tools .r {
            float: right;
        }
        table {
            width: 100%;
            color: white;
        }
        table th {
            color: #a2cbf3;
        }
        table td {
            text-align: center;
            cursor: default;
        }
        table td.today {
            background: #cc2951;
            color: white;
        }
    </style>
< body >

    <div id="calendar">

        <div id="todayTime"></div>
        <div id="todayDate"></div>

        <div id="tools">
            <div class="l">
                <select id="selectYear"></select> 年
                <select id="selectMonth"></select> 月
            </div>
            <div class="r">
                <span id="prevMonth">∧</span>
                <span id="nextMonth">∨</span>
            </div>
        </div>

        <table id="showTable">
            <thead>
                <tr>
                    <th>日</th>
                    <th>一</th>
                    <th>二</th>
                    <th>三</th>
                    <th>四</th>
                    <th>五</th>
                    <th>六</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
         <script>
            var text1 = document.getElementById('text1');
            calendar();
            function calendar() {
                var calendarElement = document.getElementById('calendar');
                var todayTimeElement = document.getElementById('todayTime');
                var todayDateElement = document.getElementById('todayDate');
                var selectYearElement = document.getElementById('selectYear');
                var selectMonthElement = document.getElementById('selectMonth');
                var showTableElement = document.getElementById('showTable');
                var prevMonthElement = document.getElementById('prevMonth');
                var nextMonthElement = document.getElementById('nextMonth');
                calendarElement.style.display = 'block';
                var today = new Date();
                var showYear = today.getFullYear();
                var showMonth = today.getMonth();
                updateTime();
                todayDateElement.innerHTML = getDate(today);
                for (var i=1970; i<2020; i++) {
                    var option = document.createElement('option');
                    option.value = i;
                    option.innerHTML = i;
                    if ( i == showYear ) {
                        option.selected = true;
                    }
                    selectYearElement.appendChild(option);
                }
                for (var i=1; i<13; i++) {
                    var option = document.createElement('option');
                    option.value = i - 1;
                    option.innerHTML = i;
                    if ( i == showMonth + 1 ) {
                        option.selected = true;
                    }
                    selectMonthElement.appendChild(option);
                }
                showTable();
                selectYearElement.onchange = function() {
                    showYear = this.value;
                    showTable();
                    showOption();
                }
                selectMonthElement.onchange = function() {
                    showMonth = Number(this.value);
                    showTable();
                    showOption();
                }
                prevMonthElement.onclick = function() {
                    showMonth--;
                    showTable();
                    showOption();
                }
                nextMonthElement.onclick = function() {
                    showMonth++;
                    showTable();
                    showOption();
                }
                function updateTime() {
                    var timer = null;
                    var today = new Date();
                    todayTimeElement.innerHTML = getTime(today);
                    timer = setInterval(function() {
                        var today = new Date();
                        todayTimeElement.innerHTML = getTime(today);
                    }, 500);
                }
                function showTable() {
                    showTableElement.tBodies[0].innerHTML = '';
                    var date1 = new Date(showYear, showMonth+1, 1, 0, 0, 0);
                    var date2 = new Date(date1.getTime() - 1);
                    var showMonthDays = date2.getDate();
                    date2.setDate(1);
                    var showMonthWeek = date2.getDay();
                    var cells = 7;
                    var rows = Math.ceil( (showMonthDays + showMonthWeek) / cells );
                    for ( var i=0; i<rows; i++ ) {
                        var tr = document.createElement('tr');
                        for ( var j=0; j<cells; j++ ) {
                            var td = document.createElement('td');
                            var v = i*cells + j - ( showMonthWeek - 1 );
                            if ( v > 0 && v <= showMonthDays  ) {
                                if ( today.getFullYear() == showYear && today.getMonth() == showMonth && today.getDate() == v ) {
                                    td.className = 'today';
                                }
                                td.innerHTML = v;
                            } else {
                                td.innerHTML = '';
                            }
                            td.ondblclick = function() {
                                calendarElement.style.display = 'none';
                                text1.value = showYear + '年' + (showMonth+1) + '月' + this.innerHTML + '日';
                            }
                            tr.appendChild(td);
                        }
                        showTableElement.tBodies[0].appendChild(tr);
                    }
                }
                function showOption() {
                    var date = new Date(showYear, showMonth);
                    var sy = date.getFullYear();
                    var sm = date.getMonth();
                    console.log(showYear, showMonth)
                    var options = selectYearElement.getElementsByTagName('option');
                    for (var i=0; i<options.length; i++) {
                        if ( options[i].value == sy ) {
                            options[i].selected = true;
                        }
                    }
                    var options = selectMonthElement.getElementsByTagName('option');
                    for (var i=0; i<options.length; i++) {
                        if ( options[i].value == sm ) {
                            options[i].selected = true;
                        }
                    }
                }
            }
            function getTime(d) {
                return [
                    addZero(d.getHours()),
                    addZero(d.getMinutes()),
                    addZero(d.getSeconds())
                ].join(':');
            }
            function getDate(d) {
                return d.getFullYear() + '年'+ addZero(d.getMonth() + 1) +'月'+ addZero(d.getDate()) +'日 星期' + getWeek(d.getDay());
            }
            function addZero(v) {
                if ( v < 10 ) {
                    return '0' + v;
                } else {
                    return '' + v;
                }
            }
            function getWeek(n) {
                return '日一二三四五六'.split('')[n];
            }
        }
    </ script>
    </div>

</body>
</html>
