# Dolphin-Agency-Rolette-new
Diaa
<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>عجلة الأسماء</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>عجلة الأسماء</h1>
        
        <div id="wheel" class="wheel"></div>

        <h2>اختيار ترتيب السحب:</h2>
        <select id="orderSelect">
            <option value="asc">من 1 إلى 10</option>
            <option value="desc">من 10 إلى 1</option>
        </select>

        <button id="spinButton">دور العجلة</button>
        <input type="text" id="nameInput" placeholder="أضف اسمًا">
        <button id="addButton">أضف</button>
        <button id="setLastTenButton">تحديد آخر 10 أسماء</button>
        <button id="resetButton">إعادة تعيين</button>

        <h2>الأسماء المدخلة:</h2>
        <ul id="nameList"></ul>

    </div>
    <script src="script.js"></script>
</body>
</html>

const spinButton = document.getElementById('spinButton');
const nameInput = document.getElementById('nameInput');
const addButton = document.getElementById('addButton');
const nameList = document.getElementById('nameList');
const resetButton = document.getElementById('resetButton');
const setLastTenButton = document.getElementById('setLastTenButton');  // زر لتحديد آخر 10 أسماء
const orderSelect = document.getElementById('orderSelect');  // قائمة لاختيار ترتيب سحب الأسماء العشرة الأخيرة

let allNames = [];  // قائمة تحتوي على جميع الأسماء المدخلة
let lastTenNames = [];  // آخر 10 أسماء سيتم سحبها
let otherNames = [];  // الأسماء التي سيتم سحبها قبل الأسماء العشرة الأخيرة

// إضافة اسم جديد
addButton.addEventListener('click', () => {
    const name = nameInput.value.trim();
    if (name) {
        allNames.push(name);  // إضافة الاسم إلى القائمة الكبيرة

        // تحديث القائمة
        updateList();
        nameInput.value = '';  // إعادة تعيين حقل الإدخال
    }
});

// تحديد آخر عشرة أسماء
setLastTenButton.addEventListener('click', () => {
    if (allNames.length > 10) {
        const selectedNames = allNames.slice(-10);  // أخذ آخر 10 أسماء من القائمة
        lastTenNames = selectedNames;
        otherNames = allNames.slice(0, allNames.length - 10);  // الأسماء التي سيتم سحبها أولًا
        alert('تم تحديد آخر عشرة أسماء للاختيار منهم');
        updateList();  // تحديث القائمة بعد تحديد الأسماء
    } else {
        alert('يجب أن يكون لديك أكثر من 10 أسماء في القائمة');
    }
});

// تدوير العجلة واختيار اسم عشوائي
spinButton.addEventListener('click', () => {
    if (otherNames.length > 0 || lastTenNames.length > 0) {
        let order = orderSelect.value;  // الحصول على ترتيب السحب من القائمة المنسدلة (من 10 إلى 1 أو العكس)

        // دمج الأسماء التي لم يتم سحبها مع الأسماء العشرة الأخيرة وفقًا للترتيب الذي اخترته
        let finalList = [...otherNames];
        if (order === "desc") {
            lastTenNames.reverse();  // عكس ترتيب الأسماء العشرة الأخيرة إذا كان من 10 إلى 1
        }

        finalList = finalList.concat(lastTenNames);  // إضافة الأسماء العشرة الأخيرة إلى النهاية

        // اختيار اسم عشوائي
        const randomIndex = Math.floor(Math.random() * finalList.length);
        const selectedName = finalList[randomIndex];

        alert(`الاسم الذي تم اختياره هو: ${selectedName}`);
        updateList();  // تحديث القائمة
    } else {
        alert('يرجى إضافة الأسماء أولاً.');
    }
});

// إعادة تعيين العجلة
resetButton.addEventListener('click', () => {
    allNames = [];
    lastTenNames = [];
    otherNames = [];  // مسح جميع الأسماء
    updateList();
});

// تحديث قائمة الأسماء
function updateList() {
    nameList.innerHTML = '';
    allNames.forEach(name => {
        const li = document.createElement('li');
        li.textContent = name;
        nameList.appendChild(li);
    });
}
