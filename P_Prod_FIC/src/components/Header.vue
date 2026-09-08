<script setup>



</script>

<template>

    <header>
        <div>
            <div class="logo">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" width="32" height="32" class="rounded-lg">
                    <rect width="32" height="32" fill="#FF0000" />
                    <path fill="#FFFFFF" d="M12 5h8v7h7v8h-7v7h-8v-7H5v-8h7z" />
                </svg>
                <p>SwissFinance</p>
            </div>

            <ul>
                <li>Dashboard</li>
                <li>Calculator</li>
                <li>Planning</li>
                <li>Resources</li>
                <li>About</li>
            </ul>
        </div>

        <div class="language-select">
            <svg xmlns="http://w3.org" viewBox="0 0 24 24" width="18" height="18" fill="none" stroke="currentColor"
                stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
                <circle cx="12" cy="12" r="10" />
                <path d="M2 12h20" />
                <path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z" />
            </svg>
            <select name="" id="">
                <option value="">English</option>
                <option value="">Français</option>
                <option value="">Deutsch</option>
                <option value="">Italiano</option>
            </select>
        </div>
    </header>

</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap');

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 80px;
    background-color: #ffffff;
    font-family: "Inter", sans-serif;
    font-optical-sizing: auto;
    font-style: normal;
    padding: 0 18px;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.15);
}

p {
    font-size: 20px;
    font-weight: 600;
}

header>div {
    display: flex;
    align-items: center;
    height: 100%;

}

header>div:first-child {
    gap: 70px;
    display: flex;
    align-items: center;
height: 100%;
}

ul {
    display: flex;
    list-style: none;
    gap: 40px;
    padding: 0;
    margin: 0;
    height: 100%;

}

ul>li {
    cursor: pointer;
    font-size: 16px;
    font-weight: 500;
    height: 100%;
    display:flex;
    justfy-content:center;
    align-items:center;
}
li:hover {
    color: #FF0000;
    border-bottom:2px solid #FF0000
}

.logo {
    display: flex;
    align-items: center;
    gap: 10px;
}

.language-select {
    height:20px;
    padding: 10px 25px;
    border: none;
    border-left: 1px solid rgb(184, 184, 184);
    gap: 5px;

}

select {
    border: none;
    outline: none;
    background: transparent;
}

option {}
</style>
