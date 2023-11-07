<template>
    <div :class="{'chessboard-and-sidebar': true, 'mobile': isMobile}">
        <div :class="chess-board">
            <h2>
                Chess Board
            </h2>
            <div
            v-for="(row, rowIndex) in board"
            :key="rowIndex"
            class="row">
                <div
                v-for="(square, columnIndex) in row"
                :key="columnIndex"
                :class="{'square': true, 'active': activeElement === square}"
                @click="addClickedPosition(square)">
                    <span 
                    class="letter"
                    v-if="rowIndex === 7">
                        {{ this.columns[columnIndex] }}
                    </span>
                    <span 
                    class="number"
                    v-if="columnIndex === 0">
                        {{ this.rows[rowIndex] }}
                    </span>
                </div>
            </div>
        </div>
        <div class="side-bar">
            <h2>Click Order</h2>
            <ol>
                <li 
                v-for="(position, positionAddedIndex) in clickedPositions"
                :key="positionAddedIndex"
                class="list">
                    {{ position }}
                </li>
            </ol>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            activeElement: null,
            clickedPositions: [],
            board: [],
            columns: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'],
            rows: [1, 2, 3, 4, 5, 6, 7, 8],
            boardFlip: false,
            isMobile: false,
        }
    },
    created() {
        this.createBoard(); 
        this.checkIfMobile();
        window.addEventListener('resize', this.checkIfMobile);
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.checkIfMobile);
    },
    methods: {
        createBoard() {
            if(!this.boardFlip) {
                let rows = this.rows.reverse();

                for(let i = 0; i < rows.length; i++) {
                    let row = [];
                    for(let j = 0; j < this.columns.length; j++) {
                        row.push(this.columns[j] + rows[i]);
                    }
                    this.board.push(row);
                }
            } else {
                let columns = this.columns.reverse();

                for(let i = 0; i < this.rows.length; i++) {
                    let row = [];
                    for(let j = 0; j < columns.length; j++) {
                        row.push(columns[j] + this.rows[i]);
                    }
                    this.board.push(row);
                }
            }
        },
        addClickedPosition(square) {
            this.clickedPositions.push(square);
            this.activeElement = square;
        },
        checkIfMobile() {
            this.isMobile = window.innerWidth < 515;
        }
    }
}
</script>

<style scoped>

.chessboard-and-sidebar {
    display: flex;
    justify-content: space-between;
    max-width: 100%;
    margin: 0 20px;
}

.chessboard-and-sidebar.mobile {
    flex-direction: column;
    margin: 0 0;
}

.side-bar {
    min-width: fit-content;
}

.chess-board {
    display: flex;
    flex-direction: column;
    width: 100%;
    max-width: fit-content;
}

.row {
    max-width: fit-content;
    display: flex;
}

.square {
    width: 5vw;
    min-width: 44px;
    aspect-ratio: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    font-size: 12px;
    font-weight: bold;
}

.chessboard-and-sidebar.mobile .square {
    width: calc(100vw/8);
    min-width: unset;
}

.square.active {
    background: yellow !important;
}

.square.active span {
    display: none;
}

.row:nth-child(2n+1) .square:nth-child(2n) {
    background: #769656;
    color: #eeeed2;
}

.row:nth-child(2n+1) .square:nth-child(2n+1) {
    color: #769656;
    background: #eeeed2;
}

.row:nth-child(2n) .square:nth-child(2n) {
    color: #769656;
    background: #eeeed2;
}

.row:nth-child(2n) .square:nth-child(2n+1) {
    background: #769656;
    color: #eeeed2;
}

.square:hover {
    cursor: pointer;
}

.square .letter {
    position: absolute;
    bottom: 1px;
    right: 4px;
    font-size: 13px;
}

.square .number{
    position: absolute;
    top: 5px;
    left: 4px;
}
</style>