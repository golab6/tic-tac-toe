<script lang="ts">
    import { draw } from "svelte/transition";

    // Skrypt do strony
    let boardState = [['', '', ''], ['', '', ''], ['', '', '']];

    let turn = "O";
    let counter = 0;
    let gameState ="";

    function checkWin() {
        for (let i = 0; i <3; i +=1){
            if(boardState[0][i] !== "" && boardState[0][i] === boardState[1][i] && boardState[1][i] === boardState[2][i]){
                return true;
            }
        }

        for (let i = 0; i <3; i +=1){
            if(boardState[i][0] !== "" && boardState[i][0] === boardState[i][1] && boardState[i][1] === boardState[i][2]){
                return true;
            }
        }

        if(boardState[0][0] !== "" && boardState[1][1] === boardState[0][0] && boardState[1][1] === boardState[2][2]){
                return true;
            }
        

        if(boardState[0][2] !== "" && boardState[0][2] === boardState[1][1] && boardState[1][1] === boardState[2][0]){
            return true;
        }
        return false
        
    }

    function markSquare(x: number, y: number) {
        if (boardState[y][x] === ""){
            boardState[y][x] = turn;
            if (counter % 2===0){
                turn = "X"
            } else{
                turn = "O"
            }
            
        }
        const isWin = checkWin();
        if (isWin){
            if (counter % 2 === 0){
                gameState = "O"
            }
            else{
                gameState = "X"
            }
        }
        console.log(checkWin())
        counter += 1
        if (counter === 9){
            gameState = "draw"
        }
        console.log(gameState)
    }

    function resetGame(){
        boardState = [['', '', ''], ['', '', ''], ['', '', '']];
        counter = 0;
        gameState = ""
    }
</script>



<main>
    <!-- Struktura strony -->
    <table>
        {#each [0, 1, 2] as y}
            <tr>
                {#each [0, 1, 2] as x}
                    <td class="cell" on:click={() => {markSquare(x, y)}}>
                        {boardState[y][x]}
                    </td>
                {/each}
            </tr>
        {/each}
    </table>

    {#if gameState ==="O"}
        O wins
    {:else if gameState === "X"}
        X wins
    {:else if gameState === "draw"}
        draw
    {:else}
        Turn:
        {#if counter%2 === 0}
            O
        {:else}
            X
        {/if}
    {/if}

    <button on:click={()=>{resetGame()}}>Play again</button>
</main>

<style>
    /* Styl strony */
    .cell {
        width: 100px;
        height: 100px;
        background-color: aqua;
        color: black;
        text-align: center;
        font-size: 40px;
    }
</style>