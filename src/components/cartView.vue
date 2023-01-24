/* eslint-disable */
<template>
    <div class="cart">
    <div id="title">Koszyk</div>
    <div id="content">
        <table id="receipt">
            <tr id="receiptRow">
                <td>    
                    LP
                </td>
                <td class="Nazwa">
                    NAZWA
                </td>
                <td class="Ilosc">
                    ILOŚĆ
                </td>
                <td class="Cena">
                    CENA
                </td>
                <td class="Suma">
                    SUMA
                </td>
                
            </tr>
            <div ref= "articles"></div>
            <tr id="totalCena">
                <td style="border:none"></td>
                <td style="border:none"></td>
                <td style="border:none"></td>
                <td style="text-align:right">
                    RAZEM:
                </td>
                <td id="c">

                </td>
            </tr>
        </table>
    </div>
  
</div>
</template>

<script>

   
    export default {
        name: 'articles-component',
        data() {
            return {
                articles: [],
                index: '',
                name: '',
                quantity: '',
                price: '',
                sum: ''
            }
        }
        // ,
        // methods: {
        //     addArticleToCart(bread) {
        //         var newArticle = {
        //             index:1,
        //             name:bread.name,
        //             quantity:2,
        //             price:bread.price,
        //             sum:price*quantity
        //         };
        //     }
        // }
    }


 
</script>

<style>
.upbar{
    width: 100%;
    background-color: #bb7900;
    position:fixed;
    top:0;
    left:0;
    height: 80px;
    z-index: 8;
}

.cart{
    width: 100%;
    background-color: #f7f5f1;
    color:black;
    /* position:fixed;
    top:0;
    left:0;
    height: 80px;
    z-index: 8; */
}


#title, #content{
    margin: auto;
    width: 50%;
    padding: 10px;
    border-top: 2px solid grey;
    border-bottom: 2px solid grey;
}

#title{
    width: 50%;
    border-top: 2px solid grey;
    text-align: center;
    font-size: xx-large;
}

#receipt{
    margin:auto;
    width:100%;
    table-layout: fixed;
}
.Nazwa{
    
    text-align:left;
}
.Ilosc, .Cena, .Suma{
    text-align:right;
}
#receipt td{
    border: 1px solid black;
    word-break:break-all;
}

.tableData
{
    border-left: 1px solid gray;
    padding:5px;
}

#nowaPozycjaTable{
    border:none;
}

#c
{
    color: blue;
    text-align:right;}


table tr:not(:first-child)
{
    cursor: pointer;transition: all .25s ease-in-out;
}
table tr:not(:first-child):hover
{
    background-color: #ddd;
}

#errorsHere{
    height: 20px;
    position: relative;
    margin-left: 30%;
    width: 40%;
    text-align: center;
    color: red;
}

#error{
    align-items: center;
    height: 50px;
}

#btnReset{
    position: relative;
    bottom: 10px;
    margin-left: 45%;
}
</style>