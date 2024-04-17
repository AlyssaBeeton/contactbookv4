<script>

import { RouterLink } from 'vue-router'

</script>

<template>

    <header>
        <nav>
            <RouterLink to="/">Contacts List</RouterLink>

            <ul>
                <li>
                    <RouterLink to="/">Home</RouterLink>
                </li>
                <li>
                    <RouterLink to="/contact">Contact</RouterLink>
                </li>
            </ul>
        </nav>
    </header>

</template>

<style scoped>

header{
    background: #EFEFEF;
    padding:20px 60px;
    max-height: 85px;
}

nav{
    display: flex;
    align-items: center;
    justify-content: space-between;
}

nav ul{
    list-style: none;
    margin:0;
    padding:0;
    display: flex;
    gap: 60px;
}

nav a{
    text-decoration: none;
}

nav ul a.portfolio-menu-active{
    text-decoration: underline;
    font-weight: bold;
}

</style>