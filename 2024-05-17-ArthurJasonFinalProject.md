---
layout: default
title: Arthur and Jason part for the Final Project
hide: true
permalink: /arthurjasonproject
---

<head>
    <style>
        .box {
            background-color: #202020; /* #202020 */
            padding: 6px;
            border: 4px solid black;
        }
        .box h2 {
            font-size: 24;
            text-align: center;
        }
        .boxcontainer {
            display: inline-block;
            justify-content: center;
            align-items: center; 
            text-align: center;
        }
        .boxcontainer h2 {
            font-size: 24;
            text-align: center;
        }
        .buttons {
            display: flex;
            gap: 10px;
        }
        .smallbox {
            width: 90px;
            height: 90px;
            display: inline-block;
            text-align: center;
            border: 2px solid black;
            margin: 0;
            font-size: 14px;
            color: black;
        }
        #mistyrose {
            background-color: #FFE4E1;
        }
        #aquamarine {
            background-color: #7FFFD4;
        }
    </style>
</head>

<body>
    <h1>Matrix Generator</h1>
    <h3>By Arthur Liu and Jason Gao</h3><br>
    <div class="box">
        <!-- inputs for dimensions -->
        <h2>Dimension and Bounds of Matrix</h2>
        Row Size: <input id="rowSize" placeholder="10"><br>
        Column Size: <input id="colSize" placeholder="10"><br>
        Lower Bound of Randomization: <input id="lower" placeholder="0"><br>
        Upper Bound of Randomization: <input id="upper" placeholder="99"><br>
        <button id="random" onclick="randomize(initMatrix())">Generate Random</button>
    </div><br>
    <div class="box">
        <!-- table for the matrix -->
        <h2>Matrix</h2>
        <div id="tablematrix"></div>
    </div><br>
    <div class="box">
        <h2>Options to Sort and Search Matrix</h2>
        <!-- options to sort/search matrix -->
        <button id="sortrow" onclick="sortrow()">Sort Row</button>
        <button id="sortcol" onclick="sortcol()">Sort Column</button>
        <!-- input for value for target -->
        <button id="search" onclick="search()">Search</button>
        <input id="target" placeholder="Target"><br>
    </div><br>
    <div class="box">
        <!-- <div class="boxcontainer"> -->
            <!-- colors for search -->
            <h2>Color of Highlight for Search</h2>
            <div class="buttons">
                <button id="mistyrose" class="smallbox">Misty Rose</button>
                <button id="aquamarine" class="smallbox">Aquamarine</button>
                <button id="" class="smallbox">3</button>
                <button id="" class="smallbox">4</button>
            </div>
        <!-- </div> -->
    </div>
</body>

<script>
    // make blank 5x5 matrix
    function initMatrix() {
        let rows = 10;
        let cols = 10;
        let matrix = new Array();
        if (document.getElementById("rowSize").value.length !== 0) {
            rows = parseInt(document.getElementById("rowSize").value, 10)
            if (rows < 1) {
                alert("Row size needs to be at least 1");
                return;
            }
        }
        if (document.getElementById("colSize").value.length !== 0) {
            cols = parseInt(document.getElementById("colSize").value, 10)
            if (cols < 1) {
                alert("Column size needs to be at least 1");
                return;
            }
        }

        for (let i = 0; i < rows; i++) {
            matrix.push(new Array(cols).fill(0));
        }
        return matrix;
    }

    // randomize and update matrix
    function randomize(matrix) {
        let lowerRand = 0;
        let upperRand = 99;
        const table = document.createElement('table');
        if (document.getElementById("lower").value.length !== 0) { // or make a function that grabs input, save as var, and return them
            lowerRand = parseInt(document.getElementById("lower").value, 10);
        }
        if (document.getElementById("upper").value.length !== 0) {
            upperRand = parseInt(document.getElementById("upper").value, 10);
        }
        if (lowerRand > upperRand) {
            alert("Invalid random bounds")
            return;
        }
        
        document.getElementById('tablematrix').innerHTML = "";
        matrix.forEach((row, rowIndex) => {
            const tr = document.createElement('tr');
            row.forEach((value, colIndex) => {
                const td = document.createElement('td');
                randomizedValue = lowerRand + Math.floor(Math.random() * (upperRand - lowerRand + 1))
                matrix[rowIndex][colIndex] = randomizedValue
                td.textContent = randomizedValue
                tr.appendChild(td);
            })
            table.appendChild(tr);
        })
        
        document.getElementById('tablematrix').appendChild(table);
    }

    // sorting algorithm
    function sortAlg(array) {
        for (let i = 1; i < array.length; i++) {
            for (let j = 0; j < i; j++) {
                if (array[i] < array[j]) { // if later element is less than former element --> swap
                    array.splice(j, 0, array[i]);
                    array.splice(i + 1, 1);
                    break;
                }
            }
        }
    }
    
    // sort matrix row
    function sortrow() {
        // create an empty array to add the matrix row values in later
        var rowarray = [];

        // must let code know that tablematrix is a table so we can iterate through it
        matrix = document.querySelector('#tablematrix table');

        // for each row, store each value, sort it numerically, and then update row with sorted values
        Array.from(matrix.rows).forEach((row, rowIndex) => {
            rowarray = Array.from(row.cells).map(cell => parseInt(cell.textContent, 10));
            sortAlg(rowarray);
            rowarray.forEach((value, colIndex) => {
                row.cells[colIndex].textContent = value;
            });
        })

        // call search
        search();
    }

    // sort matrix col
    function sortcol() {
        // grab matrix dimensions
        let rownumber = 10;
        let colnumber = 10;
        if (document.getElementById("rowSize").value.length !== 0) { 
            rownumber = parseInt(document.getElementById("rowSize").value, 10);
        }
        if (document.getElementById("colSize").value.length !== 0) {
            colnumber = parseInt(document.getElementById("colSize").value, 10);
        }
        
        // grab matrix
        const matrix = document.querySelector('#tablematrix table');
        const matrixlist = [];
        Array.from(matrix.rows).forEach(row => {
            const rowValue = [];
            Array.from(row.cells).forEach(cell => {
                rowValue.push(parseInt(cell.textContent, 10))
            });
            matrixlist.push(rowValue);
        });
        
        // sort columns
        for (let bottomcolindex = 0; bottomcolindex < colnumber; bottomcolindex++) {
            let colarray = [];
            for (let bottomrowindex = 0; bottomrowindex < rownumber; bottomrowindex++) {
                colarray.push(matrixlist[bottomrowindex][bottomcolindex])
            }
            sortAlg(colarray);
            console.log("first for ran")
            for (let bottomrowindex = 0; bottomrowindex < rownumber; bottomrowindex++) {
                matrixlist[bottomrowindex][bottomcolindex] = colarray[bottomrowindex];
            }
        }

        // update matrix
        Array.from(matrix.rows).forEach((row, rowIndex) => {
            Array.from(row.cells).forEach((cell, colIndex) => {
                cell.textContent = matrixlist[rowIndex][colIndex];
            });
        });

        // call search
        search();
    }

    // determine color for search highlight
    document.addEventListener("DOMContentLoaded", function() {
        const buttons = document.querySelectorAll('.smallbox')
        buttons.forEach(button => {
            button.addEventListener('click', function() {
                search.call(this);
            });
        });
    });

    // search
    function search() {
        var buttonid = this.id;
        let color = "#FFE4E1";
        if (buttonid === "mistyrose") {
            color = "#FFE4E1";
        }
        else if (buttonid === "aquamarine") {
            color = "#7FFFD4";
        }
        matrix = document.querySelector('#tablematrix table');
        Array.from(matrix.rows).forEach((row, rowIndex) => {
            Array.from(row.cells).forEach((value, colIndex) => {
                value.style.backgroundColor = "";
                value.style.color = "";
                if (parseInt(value.textContent, 10) === parseInt(document.getElementById("target").value, 10)) {
                    value.style.backgroundColor = color;
                    value.style.color = "black";
                }
            })
        })
    }
    
    randomize(initMatrix());
</script>