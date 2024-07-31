<template>
  <section class="simon">
    <Sector id="1" :handlerClick="handlerClick" :active="active" />
    <Sector id="2" :handlerClick="handlerClick" :active="active" />
    <Sector id="3" :handlerClick="handlerClick" :active="active" />
    <Sector id="4" :handlerClick="handlerClick" :active="active" />

    <Center
      :record="record"
      :level="level"
      :start="start"
    />
  </section>
</template>

<script lang="ts" setup>
  import { ref } from 'vue'
  import Center from '@/components/Center.vue'
  import Sector from '@/components/Sector.vue'

  document.title = 'Simon';
  const host = 'https://s3.amazonaws.com/freecodecamp/'
  // Класс для анимации
  const active = ref<number | null>(null)
  // Ввод пользователя
  const input = ref<number[]>([])
  // Сгенерир. мелодия
  const melody = ref<number[]>([])
  // Кол-во мелодий - увелич. после победы
  const length = ref(1)
  // Рекорд
  const record = ref(0)
  // Уровень
  const level = ref(0)
  // Заблок./разблок. дисплей
  const display = ref(false)

  if (localStorage.getItem('simon')) {
    record.value = Number(localStorage.getItem('simon'))
  }

  const music = ref<{[ key: number]: HTMLAudioElement }>({
    1: new Audio(`${ host }simonSound1.mp3`),
    2: new Audio(`${ host }simonSound2.mp3`),
    3: new Audio(`${ host }simonSound3.mp3`),
    4: new Audio(`${ host }simonSound4.mp3`)
  })

  /* Обрабатываем нажание на кнопку */
  const handlerClick = (e: any) => {
    if (display.value) {
      const id = e.target.id
      input.value.push(id)
      playSong(id)

      // Проверяем завершил ли пользователь ввод
      if (input.value.length == length.value) {
        checkInput()
      }
    }
  }

  /* Проигрываем мелодию */
  const playMelody = () => {
    display.value = false
    let timeEndAnimate = 0
    for (let i=0; i<melody.value.length; i++) {
      const id = melody.value[i]
      const time = 1000 * (i + 1)
      timeEndAnimate = time
      setTimeout(() => { 
        playSong(id) 
      }, time)
    }
    unlockButtons(timeEndAnimate)
  }

  /* Генерируем мелодию */
  const setMelody = () => {
    const result = []
    for (let i=0; i<length.value; i++) {
      result.push(rand(1, 4))
    }
    return result
  }

  /* Проигрываем звук и анимируем кнопку */
  const playSong = (id: number) => {
    music.value[id].play()
    active.value = id
    setTimeout(() => { 
      active.value = null
    }, 500)
  }

  /* Запустить игру */
  const start = () => {
    reset()
    melody.value = setMelody()
    playMelody()
  }

  /* Проверяем пользовательский ввод */
  const checkInput = () => {
    let result = []
    for (let i=0; i<melody.value.length; i++) {
      const songRobot = melody.value[i]
      const songUser = input.value[i]
      if (songRobot == songUser) {
        result.push(songUser)
      }
    }

    /* Если мелодия верна */
    if(result.length === melody.value.length) {
      level.value += 1
      length.value += 1
      melody.value = setMelody()
      playMelody()
    } else {
      setRecord(level.value)
      reset()
    }
    input.value = []
  }

  /* Случайное целое число от min до max, включая min и max */
  const rand = (min: number, max: number) => {
    return Math.floor(Math.random() * (max - min + 1) + min)
  }

  /* Установить новый рекорд */
  const setRecord = (value: number) => {
    if (value > record.value) {
      record.value = value
      localStorage.setItem('simon', String(record.value))
    }
  }

  /* Сбросить параметры игры */
  const reset = () => {
    length.value = 1
    level.value = 0
    display.value = false
  }

  /* Разблокировать кнопки */
  const unlockButtons = (time: number) => {
    setTimeout(() => { 
      display.value = true
    }, time)
  }
</script>

<style scoped>
  .simon {
    background: rgb(63, 63, 63);
    border: 20px solid rgb(63, 63, 63);
    margin: 20px auto;
    width: 370px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    border-radius: 50%;
  }
</style>
