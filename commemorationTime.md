<html>
    <head>
        <script src="https://code.jquery.com/jquery-3.5.1.js" integrity="sha256-QWo7LDvxbWT2tbbQ97B53yJnYU3WhH/C8ycbRAkjPDc=" crossorigin="anonymous"></script>
        <style type="text/css">
            h2#show {
                margin: 30px auto;
                text-align: center;
            }
        </style>
    </head>
    <body>
        <h2 id="show"></h2>
        <h2 id="marriage"></h2>
        <script>
            $(function() {
                let from = new Date('2016-12-15T18:00:00');
                let to = new Date();
                let result = ((to - from) / 1000 / 60 / 60 / 24) >>> 0;
                let show = document.getElementById('show');
                show.innerHTML = "自段宏俊与马晓君恋爱已" + result + "天";
                let start = new Date('2021-05-23T00:00:00');
                let now = new Date();
                let r = ((now - start) / 1000 / 60 / 60 / 24) >>> 0;
                $('#marriage').text("自段宏俊与马晓君结婚已" + r + "天");
            });
        </script>
    </body>
</html>