# new5
<!DOCTYPE html>
<html>
<head>
<title>Smart Product Recommendation System</title>

<style>

body{
font-family:Arial;
margin:0;
background:#f4f6f9;
}

header{
background:#2563eb;
color:white;
padding:15px;
font-size:22px;
text-align:center;
position:relative;
}

#historyBtn{
position:absolute;
right:20px;
top:15px;
background:#16a34a;
}

.container{
padding:20px;
}

.page{
display:none;
}

input{
padding:10px;
width:250px;
margin:5px;
border-radius:5px;
border:1px solid #ccc;
}

button{
padding:10px 15px;
border:none;
background:#2563eb;
color:white;
border-radius:5px;
cursor:pointer;
margin:5px;
}

button:hover{
background:#1e40af;
}

.location-buttons button{
background:#16a34a;
}

.products{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(230px,1fr));
gap:20px;
}

.card{
background:white;
padding:15px;
border-radius:8px;
box-shadow:0 2px 8px rgba(0,0,0,0.1);
}

.card img{
width:100%;
height:150px;
object-fit:cover;
border-radius:5px;
}

.rating{
color:#f59e0b;
}

.links a{
display:block;
margin-top:5px;
color:#2563eb;
text-decoration:none;
}

</style>
</head>

<body>

<header>
Smart Product Recommendation System
<button id="historyBtn" onclick="showHistory()">Watch History</button>
</header>

<!-- LOGIN PAGE -->

<div id="loginPage" class="container page" style="display:block">

<h2>Select Login Type</h2>

<button onclick="showUserLogin()">User Login</button>
<button onclick="showAdminLogin()">Admin Login</button>

<div id="userLogin" style="display:none">

<h3>User Login</h3>

<input id="mobile" placeholder="Enter Mobile Number">

<h3>Select Location</h3>

<div class="location-buttons">

<button onclick="setLocation('Chennai')">Chennai</button>
<button onclick="setLocation('Madurai')">Madurai</button>
<button onclick="setLocation('Trichy')">Trichy</button>
<button onclick="setLocation('Coimbatore')">Coimbatore</button>

</div>

<button onclick="login()">Enter Website</button>

</div>

<div id="adminLogin" style="display:none">

<h3>Admin Login</h3>

<input id="adminUser" placeholder="Admin Username">
<input id="adminPass" type="password" placeholder="Admin Password">

<button onclick="adminLogin()">Login Admin</button>

</div>

</div>

<!-- MAIN WEBSITE -->

<div id="mainPage" class="container page">

<h3>Welcome <span id="userMobile"></span></h3>

<p>Location: <b id="userLocation"></b></p>

<input id="searchInput" placeholder="Search product">

<button onclick="searchProduct()">Search</button>

<h3>Quick Search</h3>

<button onclick="customSearch('mobile')">Mobiles</button>
<button onclick="customSearch('skin')">Skin Products</button>
<button onclick="customSearch('furniture')">Furniture</button>

<div id="dynamicFilters"></div>

<h3>Products</h3>

<div class="products" id="productList"></div>

</div>

<!-- ADMIN PANEL -->

<div id="adminPage" class="container page">

<h2>Admin Panel</h2>

<input id="pname" placeholder="Product Name">
<input id="ptype" placeholder="Product Type">
<input id="pimage" placeholder="Image URL">
<input id="pshop" placeholder="Shop Name">
<input id="pamazon" placeholder="Amazon Link">
<input id="pflipkart" placeholder="Flipkart Link">
<input id="prating" placeholder="Rating (1-5)">
<input id="preview" placeholder="Product Review">

<br>

<button onclick="addProduct()">Add Product</button>

<button onclick="goHome()">Back to Website</button>

</div>

<script>

let selectedLocation="";
let history=[];

let products=[

{
name:"iPhone 14",
type:"mobile",
shop:"Reliance Digital",
image:"https://images.unsplash.com/photo-1598327105666",
amazon:"https://www.amazon.in",
flipkart:"https://www.flipkart.com",
rating:"4.8",
review:"Excellent camera and performance"
},

{
name:"Samsung Galaxy S23",
type:"mobile",
shop:"Poorvika Mobiles",
image:"https://images.unsplash.com/photo-1511707171634",
amazon:"https://www.amazon.in",
flipkart:"https://www.flipkart.com",
rating:"4.6",
review:"Smooth performance and great display"
},

{
name:"Face Cream",
type:"skin",
shop:"Health Store",
image:"https://images.unsplash.com/photo-1596755389378",
amazon:"https://www.amazon.in",
flipkart:"https://www.flipkart.com",
rating:"4.4",
review:"Good for dry skin"
},

{
name:"Wooden Sofa",
type:"furniture",
shop:"IKEA Store",
image:"https://images.unsplash.com/photo-1505693416388",
amazon:"https://www.amazon.in",
flipkart:"https://www.flipkart.com",
rating:"4.5",
review:"Comfortable sofa"
}

];

function showUserLogin(){
document.getElementById("userLogin").style.display="block";
document.getElementById("adminLogin").style.display="none";
}

function showAdminLogin(){
document.getElementById("adminLogin").style.display="block";
document.getElementById("userLogin").style.display="none";
}

function setLocation(city){
selectedLocation=city;
alert("Location selected: "+city);
}

function login(){

let mobile=document.getElementById("mobile").value;

if(mobile.length>=10 && selectedLocation!=""){

document.getElementById("loginPage").style.display="none";
document.getElementById("mainPage").style.display="block";

document.getElementById("userMobile").innerText=mobile;
document.getElementById("userLocation").innerText=selectedLocation;

displayProducts(products);

}
else{
alert("Enter mobile number and location");
}

}

function adminLogin(){

let user=document.getElementById("adminUser").value;
let pass=document.getElementById("adminPass").value;

if(user=="admin" && pass=="1234"){

document.getElementById("loginPage").style.display="none";
document.getElementById("adminPage").style.display="block";

}
else{
alert("Invalid admin login");
}

}

function searchProduct(){

let query=document.getElementById("searchInput").value.toLowerCase();

let result=products.filter(p=>p.name.toLowerCase().includes(query) || p.type.includes(query));

displayProducts(result);

showFilters(query);

}

function customSearch(type){

let result=products.filter(p=>p.type.includes(type));

displayProducts(result);

showFilters(type);

}

function showFilters(query){

let html="";

if(query.includes("mobile")){

html=`<h3>Select Brand</h3>
<button onclick="brandFilter('iphone')">Apple</button>
<button onclick="brandFilter('samsung')">Samsung</button>`;

}

else if(query.includes("skin")){

html=`<h3>Select Skin Type</h3>
<button>Oily Skin</button>
<button>Dry Skin</button>
<button>Normal Skin</button>`;

}

else if(query.includes("furniture")){

html=`<h3>Select Colour</h3>
<button>Brown</button>
<button>Black</button>
<button>White</button>`;

}

document.getElementById("dynamicFilters").innerHTML=html;

}

function brandFilter(brand){

let result=products.filter(p=>p.name.toLowerCase().includes(brand));

displayProducts(result);

}

function displayProducts(list){

let html="";

list.forEach(p=>{

history.push(p.name);

html+=`

<div class="card">

<img src="${p.image}">

<h4>${p.name}</h4>

<p class="rating">⭐ ${p.rating}</p>

<p><b>Review:</b> ${p.review}</p>

<p><b>Shop:</b> ${p.shop}</p>

<div class="links">

<a target="_blank" href="${p.amazon}">Buy on Amazon</a>

<a target="_blank" href="${p.flipkart}">Buy on Flipkart</a>

<a target="_blank" href="https://www.google.com/maps/search/${p.shop}">Find Shop</a>

</div>

</div>

`

});

document.getElementById("productList").innerHTML=html;

}

function showHistory(){

alert("Watch History:\n\n"+history.join("\n"));

}

function addProduct(){

let name=document.getElementById("pname").value;
let type=document.getElementById("ptype").value;
let image=document.getElementById("pimage").value;
let shop=document.getElementById("pshop").value;
let amazon=document.getElementById("pamazon").value;
let flipkart=document.getElementById("pflipkart").value;
let rating=document.getElementById("prating").value;
let review=document.getElementById("preview").value;

products.push({
name:name,
type:type,
image:image,
shop:shop,
amazon:amazon,
flipkart:flipkart,
rating:rating,
review:review
});

alert("Product Added Successfully");

}

function goHome(){

document.getElementById("adminPage").style.display="none";
document.getElementById("mainPage").style.display="block";

displayProducts(products);

}

</script>

</body>
</html>
