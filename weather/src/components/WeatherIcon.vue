<template>
    <div>
        <img :src="`https://openweathermap.org/img/wn/${icon}@2x.png`" alt="Weather icon">
    </div>
</template>

<script>
export default {
    name: 'WeatherIcon',
    props: ['icon'],
}
</script>