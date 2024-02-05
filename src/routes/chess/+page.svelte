<script>
    import Fa from "svelte-fa";
    import {
        faChessQueen as qb,
        faChessBishop as bb,
        faChessPawn as pb,
        faChessKing as kb,
        faChessKnight as nb,
        faChessRook as rb,
    } from "@fortawesome/free-solid-svg-icons";
    import {
        faChessQueen as qw,
        faChessBishop as bw,
        faChessPawn as pw,
        faChessKing as kw,
        faChessKnight as nw,
        faChessRook as rw,
    } from "@fortawesome/free-regular-svg-icons";

    const blackPieces = {
        queen: {
            symbol: qb,
            notation: "Q",
            color: "black",
        },
        bishop: {
            symbol: bb,
            notation: "B",
            color: "black",
        },
        pawn: {
            symbol: pb,
            notation: "P",
            color: "black",
        },
        king: {
            symbol: kb,
            notation: "K",
            color: "black",
        },
        rook: {
            symbol: rb,
            notation: "R",
            color: "black",
        },
        knight: {
            symbol: nb,
            notation: "N",
            color: "black",
        },
    };

    const whitePieces = {
        queen: {
            symbol: qw,
            notation: "Q",
            color: "white",
        },
        bishop: {
            symbol: bw,
            notation: "B",
            color: "white",
        },
        pawn: {
            symbol: pw,
            notation: "P",
            color: "white",
        },
        king: {
            symbol: kw,
            notation: "K",
            color: "white",
        },
        rook: {
            symbol: rw,
            notation: "R",
            color: "white",
        },
        knight: {
            symbol: nw,
            notation: "N",
            color: "white",
        },
    };

    let board = [
        [
            whitePieces.rook,
            whitePieces.knight,
            whitePieces.bishop,
            whitePieces.queen,
            whitePieces.king,
            whitePieces.bishop,
            whitePieces.knight,
            whitePieces.rook,
        ],
        [
            whitePieces.pawn,
            whitePieces.pawn,
            whitePieces.pawn,
            whitePieces.pawn,
            whitePieces.pawn,
            whitePieces.pawn,
            whitePieces.pawn,
            whitePieces.pawn,
        ],
        [null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null],
        [null, null, null, null, null, null, null, null],
        [
            blackPieces.pawn,
            blackPieces.pawn,
            blackPieces.pawn,
            blackPieces.pawn,
            blackPieces.pawn,
            blackPieces.pawn,
            blackPieces.pawn,
            blackPieces.pawn,
        ],
        [
            blackPieces.rook,
            blackPieces.knight,
            blackPieces.bishop,
            blackPieces.queen,
            blackPieces.king,
            blackPieces.bishop,
            blackPieces.knight,
            blackPieces.rook,
        ],
    ];

    // [white, black]
    let movesList = [
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],

        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],

        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],

        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],

        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],

        ["abcd", "abcd"],
        ["abcd", "abcd"],
        ["abcd", "abcd"],
    ];

    let currentTurn = "white";

    let boardRef;

    $: console.log(currentTurn, movesList.length);

    const handleDrag = (event) => {
        if (!(event instanceof DragEvent) || !event) return;
        event.dataTransfer.setData("text/plain", event.target.id);
        event.dataTransfer.effectAllowed = "move";
    };

    const handleDragEnter = (event) => {
        event.preventDefault();
        event.dataTransfer.dropEffect = "move";

        const [nextRow, nextCol] = event.target.id.split("-").map((item) => (item ? Number(item) : null));
        const [row, col] = event.dataTransfer
            .getData("text/plain")
            .split("-")
            .map((item) => (item ? Number(item) : null));

        if (row === null || col === null || nextCol === null || nextRow === null) return;
        if (row === nextRow && col === nextCol) return;

        const itemIndex = nextRow * 8 + nextCol;
        const node = boardRef.children.item(itemIndex);
        node.classList.add("selected");
    };

    const handleDragLeave = (event) => {
        event.preventDefault();
        event.dataTransfer.dropEffect = "move";

        const [row, col] = event.target.id.split("-").map((item) => (item ? Number(item) : null));
        if (row === null || col === null) return;

        const itemIndex = row * 8 + col;
        const node = boardRef.children.item(itemIndex);
        node.classList.remove("selected");
    };

    const handleDrop = (event) => {
        console.log(event);
        const data = event.dataTransfer.getData("text/plain");
        const [row, col] = data.split("-").map((item) => (item ? Number(item) : null));
        const [nextRow, nextCol] = event.target.id.split("-").map((item) => (item ? Number(item) : null));

        if (row === nextRow && col === nextCol) return;
        if (row === null || col === null) return;
        if (board?.[nextRow]?.[nextCol] !== null) return;

        const itemIndex = nextRow * 8 + nextCol;
        console.log({ itemIndex, row, col });
        const node = boardRef.children.item(itemIndex);
        node.classList.remove("selected");

        board[nextRow][nextCol] = board[row][col];
        board[row][col] = null;
        currentTurn = currentTurn === "white" ? "black" : "white";
    };


</script>

<div class="sub-container">
    <div class="board" bind:this={boardRef}>
        {#each board as row, id}
            {#each row as item, id2 (`${id}-${id2}`)}
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <div
                    id="{id}+{id2}"
                    class="square"
                    class:odd={id % 2 === 1}
                    class:even={id % 2 === 0}
                    on:drop={handleDrop}
                >
                    <!-- svelte-ignore a11y-no-static-element-interactions -->
                    <div
                        id="{id}-{id2}"
                        draggable={item ? currentTurn === item.color : false}
                        class="icon"
                        class:draggable={item ? currentTurn === item.color : false}
                        on:dragstart={handleDrag}
                        on:dragenter={handleDragEnter}
                        on:dragleave={handleDragLeave}
                        on:dragover={(e) => {
                            e.preventDefault();
                        }}
                        on:dragend={(e) => {
                            e.preventDefault();
                        }}
                        style="z-index: 2;"
                    >
                        <!-- {8*id+id2} -->
                        {#if !!item}
                            <Fa icon={item.symbol} size="3x" style="z-index: 1;" />
                        {/if}
                    </div>
                </div>
            {/each}
        {/each}
        <header>Current player: {currentTurn}</header>
    </div>

    <section class="moves-container">
        <header>Moves List</header>
        <ul class="moves">
            {#each movesList as move, id}
                <li class="move">{id + 1}. {move[0]} | {move[1]}</li>
            {/each}
        </ul>
    </section>
</div>

<style>
    :global(.selected) {
        transition: all ease-in-out 0.15s;
        background-color: goldenrod !important;
    }
    .sub-container {
        display: flex;
        gap: 2rem;
        -webkit-user-select: none; /* Safari */
        user-select: none; /* Standard syntax */
    }
    .moves-container {
        border-radius: 5px;
        padding: 1rem;
        width: 400px;
        box-shadow:
            0 10px 15px -3px rgb(0 0 0 / 0.1),
            0 4px 6px -4px rgb(0 0 0 / 0.1);
        max-height: 800px;
        overflow-y: hidden;
    }
    .moves {
        display: flex;
        flex-direction: column;
        font-size: 12px;
        gap: 0.5rem;
        list-style-type: none;
        list-style-position: inside;
        margin: 0;
        padding: 0;
        margin-top: 1rem !important;
        max-height: 100%;
        overflow-y: scroll;
        padding-right: 0.75rem;
    }
    .move {
        padding: 0.5rem;
        box-shadow:
            0 10px 15px -3px rgb(0 0 0 / 0.1),
            0 4px 6px -4px rgb(0 0 0 / 0.1);
        border-radius: 5px;
    }
    .board {
        border: 1px black solid;
        border-radius: 5px;
        display: flex;
        flex-wrap: wrap;
        max-width: 800px;
        max-height: 800px;
    }
    .square {
        min-width: 12.5%;
        max-width: 12.5%;
        aspect-ratio: 1/1;
        text-align: center;
    }
    .draggable {
        cursor: grab;
    }
    .icon {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: 100%;
    }
    .odd:nth-child(odd) {
        background-color: brown;
    }
    .even:nth-child(even) {
        background-color: brown;
    }
</style>
