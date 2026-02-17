<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>الاستعلام عن النقاط</title>
<style>
body {font-family: Arial; text-align:center; background:#f2f2f2;}
.box {background:white; padding:20px; margin:50px auto; width:300px; border-radius:10px;}
input {width:90%; padding:10px; margin:10px;}
button {padding:10px 20px; background:#2e7d32; color:white; border:none;}
.result {margin-top:20px; font-size:18px;}
</style>
</head>

<body>
<div class="box">
<h2>ادخل رقمك التعريفي</h2>
<input type="text" id="searchId" placeholder="الرقم التعريفي">
<button onclick="search()">بحث</button>
<div class="result" id="result"></div>
</div>

<script>
const sheetURL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRbG4aS3qTLCl0dRr5IwcLlI9zo7UaeOwV4ThCGl1Sn45QGYErBSdcXOU_h0-HCSxMyl8jA8yDs2ElY/pub?output=csv";

async function search(){
    let id = document.getElementById("searchId").value.trim();
    if(!id){ 
        document.getElementById("result").innerHTML = "الرجاء إدخال الرقم التعريفي";
        return;
    }

    try {
        let res = await fetch(sheetURL);
        let text = await res.text();
        let rows = text.split("\n").slice(1);

        let found = false;

        rows.forEach(r=>{
            let cols = r.split(",");
            if(cols[0] == id){
                found = true;
                document.getElementById("result").innerHTML =
                "الاسم: " + cols[1] + "<br>" +
                "النقاط المتبقية: " + cols[2] + "<br>" +
                "النقاط المخصومة: " + cols[3];
            }
        });

        if(!found){
            document.getElementById("result").innerHTML="لا توجد بيانات لهذا الرقم";
        }

    } catch (e) {
        document.getElementById("result").innerHTML="حدث خطأ في جلب البيانات";
        console.error(e);
    }
}
</script>
</body>
</html>
