<html>
    <head>
        <style type="text/css">
            h2#show {
                margin: 30px auto;
                text-align: center;
            }
        </style>
    </head>
    <body>
        <h2 id="show"></h2>
        <script>
            let from = new Date('2016-12-15T18:00:00');
            let to = new Date();
            let result = ((to - from) / 1000 / 60 / 60 / 24) >>> 0;
            let show = document.getElementById('show');
            show.innerHTML = "自段宏俊与马晓君恋爱已" + result + "天";
        </script>
    </body>
</html>