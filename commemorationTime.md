
<html>
    <head>
        <script src="https://code.jquery.com/jquery-3.5.1.js" integrity="sha256-QWo7LDvxbWT2tbbQ97B53yJnYU3WhH/C8ycbRAkjPDc=" crossorigin="anonymous"></script>
        <style type="text/css">
            li#show, li#marriage, h2#marriageDetail {
                margin: 30px auto;
                font-weight: bolder;
                font-size: larger;
            }
        </style>
    </head>
    <body>
        <ul>
            <li id="show"></li>
            <li id="marriage"></li>
        </ul>
        <script>
            $(function() {
                let from = new Date('2016-12-15T18:00:00');
                let to = new Date();
                let result = ((to - from) / 1000 / 60 / 60 / 24) >>> 0;
                $('#show').text("自段宏俊与马晓君恋爱已" + result + "天");
                fun();
                setInterval(fun, 1000);
            });
            function fun() {
                let start = new Date('2021-05-23T00:00:00');
                let now = new Date();
                let r = ((now - start) / 1000 / 60 / 60 / 24) >>> 0;
                $('#marriage').text("自段宏俊与马晓君结婚已" + trans(r) + "天" + 
                "(" + trans(now.getFullYear() - start.getFullYear()) + "年"
                + trans(now.getMonth() - start.getMonth()) + "月" + trans(now.getDay() - start.getDay()) + "日"
                + trans(now.getHours() - start.getHours()) + "时" + trans(now.getMinutes() - start.getMinutes()) + "分"
                + trans(now.getSeconds() - start.getSeconds()) + "秒" + ")");
            }
            function trans(e) {
                if (e < 10) {
                    return '0' + e;
                } else return e;
            }
        </script>
    </body>
</html>