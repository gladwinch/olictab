<template>
    <div class="bg-panel-400 h-screen flex flex-col justify-between pb-5">
        <!-- header nav -->
        <div>
            <div class="h-14 bg-panel-600 flex justify-center items-center shadow-inner cursor-pointer">
                <img class="h-5" alt="logo" src="../assets/img/logo.png" >
            </div>

            <div class="px-4 my-6">
                <img class="h-5 cursor-pointer" alt="logo" src="../assets/img/chart-pie.png" >
                <div class="my-6 rounded-lg bg-panel-200 w-5" style="height: 2px"></div> 
                <img class="h-5 my-6 cursor-pointer" alt="logo" src="../assets/img/calendar.png" >
            </div>

            <div class="border-l-2 border-primary-400 h-6 flex items-center">
                <img class="h-5 my-6 cursor-pointer" alt="logo" src="../assets/img/user.png" style="margin-left: .9rem" >
            </div>
        </div>

        <!-- footer nav -->
        <div class="h-8 w-8 mx-auto rounded-full overflow-hidden border-2 border-gray-300 object-fit-center">
            <img 
                class="h-8 cursor-pointer object-fit-cover" 
                alt="logo" src="../assets/img/person.jpg" >
        </div>
    </div>
</template>

<script>
    export default {

    }
</script>

<style>

</style>