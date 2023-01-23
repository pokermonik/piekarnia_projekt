/* eslint-disable */
<template>
    <div id="title">PARAGON 05-12-22/001</div>
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
    <div id="opcje">
        <table>
            <tr>
                <td class="tableData" style="border:none;">
                    <table id="nowaPozycjaTable">
                        <h3>Nowa pozycja:</h3>
                        <tr>
                            <td>Nazwa:</td>
                            <td><input type="text" id="nazwa"></input></td>
                        </tr>
                        <tr>
                            <td>Ilość:</td>
                            <td><input type="text" id="ilosc"></input></td>
                        </tr>
                        <tr>
                            <td>Cena:</td>
                            <td><input type="text" id="cena"></input></td>
                        </tr>
                        <tr>
                            <td><input type="button" id="submit" value="Dodaj" onClick="dodaj(
                                document.getElementById('nazwa').value,
                                document.getElementById('ilosc').value,
                                document.getElementById('cena').value)"></input></td>
                        </tr>
                    </table>
                </td>
                <td class="tableData">
                    <table id="usunPozycjeTable">
                        <h3>Usuń pozycję:</h3>
                        <tr>
                            <td>Numer na liście:</td>
                            <td><input type="number" id="numernaliscie"></input></td>
                        </tr>
                        <tr>
                            <td><input type="button" id="submit" value="Usuń" onClick="usun(
                                document.getElementById('numernaliscie').value)"></input></td>
                        </tr>
                    </table>
                </td>
                <td class="tableData">
                    <table id="edytujPozycjeTable">
                        <h3>Edytuj pozycję:</h3>
                        <tr>
                            <td>Numer na liście:</td>
                            <td><input type="number" id="numnum"></input></td>
                        </tr>
                        <tr>
                            <td>Nazwa:</td>
                            <td><input type="text" id="nazwa2"></input></td>
                        </tr>
                        <tr>
                            <td>Ilość:</td>
                            <td><input type="text" id="ilosc2"></input></td>
                        </tr>
                        <tr>
                            <td>Cena:</td>
                            <td><input type="text" id="cena2"></input></td>
                        </tr>
                        <tr>
                            <td><input type="button" id="submit" value="Edytuj" onClick="edytuj(
                                document.getElementById('numnum').value,
                                document.getElementById('nazwa2').value,
                                document.getElementById('ilosc2').value,
                                document.getElementById('cena2').value)"></input></td>
                        </tr>
                    </table>
                </td>
                <td class="tableData">
                    <table id="ZamienKolejnoscTable">
                        <h3>Zamień kolejność:</h3>
                        <tr>
                            <td>Numer elementu #1:</td>
                            <td><input type="number" id="num1"></input></td>
                        </tr>
                        <tr>
                            <td>Numer elementu #2:</td>
                            <td><input type="number" id="num2"></input></td>
                        </tr>
                        <tr>
                            <td><input type="button" id="submit" value="Zamień" onClick="zamien(
                                document.getElementById('num1').value,
                                document.getElementById('num2').value)"></input></td>
                        </tr>
                    </table>
                </td>
            </tr>
        </table>
        <div id="error"> 
            <p id="errorsHere"></p>
        <input type="button" value="Resetuj wszystko" id="btnReset" onclick="usunWszystko()"> </input ></div>
       
    
    </div>
</template>

<script>let pozycjeParagonu = document.querySelectorAll("tr.receiptRow")
    let index = 1
    let inti = 0
    let total = 0
    const d = new Date();
    let month = (d.getMonth() + 1).toString();
    let lista = []
    document.getElementById("title").innerHTML = "PARAGON " + d.getDate() + "-" + month + "-" + d.getFullYear().toString().substr(-2) + "/001"
    
    class pozycjaWMenu{
        constructor(id, nazwa, ilosc, cena, suma){
            this.id = id;
            this.nazwa = nazwa;
            this.ilosc = ilosc;
            this.cena = cena;
            this.suma = suma;
        }
    
        setId(id){
            this.id = id;
        }
    }
  
   
    
    let onload = function(){
        lista = JSON.parse(localStorage.pozycje || "[]");
        
        for(let i=0;i<lista.length;i++)
        {
            let p = new pozycjaWMenu(i+1, lista[i].nazwa, lista[i].ilosc,  lista[i].cena, parseFloat((lista[i].cena * lista[i].ilosc).toFixed(2)))
            var table = document.getElementById("receipt");
            var totalRowCount = table.rows.length;
            var row = table.insertRow(totalRowCount-1);
        
            var LPCell = row.insertCell(0);
            LPCell.className = 'LP';
            LPCell.id = 'numer';
            LPCell.innerHTML = p.id;
        
            var NazwaCell = row.insertCell(1);
            NazwaCell.className = 'Nazwa';
            NazwaCell.innerHTML = p.nazwa;
        
            var IloscCell = row.insertCell(2);
            IloscCell.className = 'Ilosc';
            IloscCell.innerHTML = p.ilosc;
        
            var CenaCell = row.insertCell(3);
            CenaCell.className = 'Cena';
            CenaCell.innerHTML = p.cena.toFixed(2) + " zł";
        
            var SumaCell = row.insertCell(4);
            SumaCell.className = 'Suma';
            SumaCell.innerHTML = p.suma.toFixed(2) + " zł";
  
            var pom= document.getElementById('ilosc').value;
  
            total += p.suma
            document.getElementById("c").innerHTML= total.toFixed(2) + " zł";
  
            NazwaCell.onmouseover = function()
    {
      NazwaCell.title = NazwaCell.innerHTML.length;
      }
        }
    }
    onload()
  
  var pobierz = document.getElementById("receipt"), rIndex;
  
  for(var i = 2; i < pobierz.rows.length; i++)
              {
                  pobierz.rows[i].onclick = function()
                  {
                      rIndex = this.rowIndex;
                      console.log(rIndex);
                      
                      document.getElementById("numnum").value = this.cells[0].innerHTML;
                      document.getElementById("nazwa2").value = this.cells[1].innerHTML;
                      document.getElementById("ilosc2").value = this.cells[2].innerHTML;
                      document.getElementById("cena2").value = this.cells[2].innerHTML;
  
                      document.getElementById("numernaliscie").value = this.cells[0].innerHTML;
  
                  };
              }
    let dodaj = function(nazwa, ilosc, cena)
    {
      var pom= document.getElementById('ilosc').value;
        //zamiana przecinków na kropki, żeby można było floaty liczyć
        let tempIlosc = parseFloat(ilosc.replace(',','.').replace(' ',''))
        let tempCena = parseFloat(cena.replace(',','.').replace(' ',''))
    
        if(tempIlosc != parseFloat(tempIlosc, 10)){
            document.getElementById("errorsHere").innerHTML="Podana ilość nie jest liczbą";
        }
        else if(tempCena != parseFloat(tempCena, 10))
        {
            document.getElementById("errorsHere").innerHTML="Podana cena nie jest liczbą";
        }
        else{//jeśli liczby są spoko
            let p = new pozycjaWMenu(lista.length+1, nazwa, tempIlosc,  tempCena, parseFloat((tempCena * tempIlosc).toFixed(2)))
            document.getElementById("errorsHere").innerHTML="";
            lista.push(p);
            localStorage.pozycje = JSON.stringify(lista);
    
            var table = document.getElementById("receipt");
            var totalRowCount = table.rows.length;
            var row = table.insertRow(totalRowCount-1);
        
            var LPCell = row.insertCell(0);
            LPCell.className = 'LP';
            LPCell.id = 'numer';
            LPCell.innerHTML = p.id;
        
            var NazwaCell = row.insertCell(1);
            NazwaCell.className = 'Nazwa';
            NazwaCell.innerHTML = p.nazwa;
        
            var IloscCell = row.insertCell(2);
            IloscCell.className = 'Ilosc';
            IloscCell.innerHTML = p.ilosc;
        
            var CenaCell = row.insertCell(3);
            CenaCell.className = 'Cena';
            CenaCell.innerHTML = p.cena.toFixed(2) + " zł";
  
            
            var SumaCell = row.insertCell(4);
            SumaCell.className = 'Suma';
            SumaCell.innerHTML = p.suma.toFixed(2) + " zł";
  
            total += p.suma
            document.getElementById("c").innerHTML= total.toFixed(2) + " zł";
  
            czysc();
        }
    }
  
    let czysc =function(){
  
      document.getElementById('nazwa').value =""
      document.getElementById('ilosc').value =""
      document.getElementById('cena').value =""
    }
    
    let usun = function(id)
    {
          if(id != parseInt(id)){
              document.getElementById("errorsHere").innerHTML="Podana wartość nie jest liczbą całkowitą!";
          }
          else
          {
              id = +id
              var ind = lista.map(function(item) { return item.id; }).indexOf(id); //index usuwanego itemu na liście
              total -= lista[ind].suma;
              document.getElementById("c").innerHTML=total.toFixed(2) + " zł";
              lista = lista.filter(function(el) { return el.id != id; }) //usunięcie go z listy
              document.getElementById("receipt").deleteRow(++ind); //usunięcie
              for (let i = 0; i < lista.length; i++)
              {
                lista[i].id = i+1;
              }
              poprawId();
              localStorage.setItem("pozycje", JSON.stringify(lista));
        
              document.getElementById('numernaliscie').value =""
     
          }
        
    }
    
    let edytuj = function(lp, nazwa, ilosc, cena){
      //weź item o podanym LP i zamień jego parametry
      let ti = parseFloat(ilosc.replace(',','.').replace(' ',''))
      let tc = parseFloat(cena.replace(',','.').replace(' ',''))
  
      if(lp != parseInt(lp)){
          document.getElementById("errorsHere").innerHTML="Podany numer nie jest liczbą całkowitą!";
      }
      else if(ti != parseFloat(ti,10)){
          document.getElementById("errorsHere").innerHTML="Podana ilość nie jest liczbą!";
      }
      else if(tc != parseFloat(tc,10)){
          document.getElementById("errorsHere").innerHTML="Podana cena nie jest liczbą!";
      }
      else{
          lista = JSON.parse(localStorage.pozycje || "[]");
          lp = parseInt(lp);
          var ix = lista.findIndex(o => o.id == lp); //index itemu na liście
          console.log("ix = " + ix) //czemu -1?
          total -= lista[ix].suma;
          
          
          lista[ix].nazwa = nazwa;
          lista[ix].ilosc = ti;
          lista[ix].cena = parseFloat(tc.toFixed(2));
          lista[ix].suma = parseFloat((ti * tc).toFixed(2))
          var table = document.getElementById("receipt")
          
          table.rows[ix+1].cells[1].innerHTML = lista[ix].nazwa
          table.rows[ix+1].cells[2].innerHTML = lista[ix].ilosc
          table.rows[ix+1].cells[3].innerHTML = lista[ix].cena.toFixed(2) + " zł";
          table.rows[ix+1].cells[4].innerHTML = parseFloat((ti * tc).toFixed(2)).toFixed(2) + " zł"
          total += parseFloat((ti * tc).toFixed(2))
          document.getElementById("c").innerHTML= total.toFixed(2) + " zł";
          document.getElementById("errorsHere").innerHTML="";
          localStorage.setItem("pozycje", JSON.stringify(lista));
  
          document.getElementById('numnum').value ="";
          document.getElementById('nazwa2').value ="";
          document.getElementById('ilosc2').value ="";
          document.getElementById('cena2').value ="";
  
          
      }
      
  }
  
  let zamien = function(num1, num2)
  {
      if(num1 != parseInt(num1)){
          document.getElementById("errorsHere").innerHTML="Pierwsza wartość nie jest liczbą całkowitą!";
      }
      else if(num2!=parseInt(num2)){
          document.getElementById("errorsHere").innerHTML="Druga wartość nie jest liczbą całkowitą!";
      }
      else{
          lista = JSON.parse(localStorage.pozycje || "[]");
          num1 = parseInt(num1);
          num2 = parseInt(num2);
          var i1 = lista.findIndex(o => o.id == num1); //index itemu1 na liście
          var i2 = lista.findIndex(o => o.id == num2); //index itemu2 na liście
          let tempC, tempI, tempSuma, tempNazwa
          
          tempC = lista[i1].cena
          tempI = lista[i1].ilosc
          tempSuma = lista[i1].suma
          tempNazwa = lista[i1].nazwa
      
          
          lista[i1].cena = lista[i2].cena
          lista[i1].ilosc = lista[i2].ilosc
          lista[i1].suma = lista[i2].suma
          lista[i1].nazwa = lista[i2].nazwa
      
          lista[i2].nazwa = tempNazwa
          lista[i2].cena = tempC
          lista[i2].ilosc = tempI
          lista[i2].suma = tempSuma
      
          var table = document.getElementById("receipt")
          
          table.rows[i1+1].cells[1].innerHTML = lista[i1].nazwa
          table.rows[i1+1].cells[2].innerHTML = lista[i1].ilosc
          table.rows[i1+1].cells[3].innerHTML = lista[i1].cena.toFixed(2) + " zł";
          table.rows[i1+1].cells[4].innerHTML = lista[i1].suma.toFixed(2) + " zł"
      
          table.rows[i2+1].cells[1].innerHTML = lista[i2].nazwa
          table.rows[i2+1].cells[2].innerHTML = lista[i2].ilosc
          table.rows[i2+1].cells[3].innerHTML = lista[i2].cena.toFixed(2) + " zł";
          table.rows[i2+1].cells[4].innerHTML = lista[i2].suma.toFixed(2) + " zł"
          localStorage.setItem("pozycje", JSON.stringify(lista));
          document.getElementById("errorsHere").innerHTML="";
  
          document.getElementById('num1').value ="";
          document.getElementById('num2').value ="";
      }
      
  }
  
    let poprawId = function(){
        var numerki = document.getElementsByClassName("LP")
        for (var i = 0; i < numerki.length; i++) {
            numerki[i].innerHTML = lista[i].id;
        }
    }
    
    let usunWszystko = function () {
      delete localStorage.pozycje
      window.location.reload()
    }</script>

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
</style>