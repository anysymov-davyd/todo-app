<template>
    <svg :style="{ fill : darkMode ? 'black' : 'white' }" class="todo__button" width="10" height="10" viewBox="0 0 10 10">
        <g clip-path="url(#clip0_8_2)">
            <path d="M10 5.71429H5.71429V10H4.28571V5.71429H0V4.28571H4.28571V0H5.71429V4.28571H10V5.71429Z"/>
        </g>
        <defs>
            <clipPath id="clip0_8_2">
                <rect width="10" height="10" fill="white"/>
            </clipPath>
        </defs>
    </svg>
</template>

<script>
    export default{
        props: {
            darkMode: Boolean
        }
    }
</script>

<style>
    .todo__button{
        background-color: rgba(0, 0, 0, 0);
        border-top-right-radius: 6px;
        border-bottom-right-radius: 6px;
        transition: .2s;
        height: 100%;
        padding: 11.75px;
        width: 16.5px;
        height: 16.5px;
        opacity: .5;
        cursor: pointer;
    }

    .todo__button:hover{
        background-color: rgba(180, 180, 180, .25);
    }

    .todo__button:active{
        background-color: rgba(180, 180, 180, .4);
    }
</style>