<template>
	<div id="app" class="container-fluid">
		<h1>Animações</h1>
		<hr>
		<!-- <b-button variant="primary" class="mb-4" @click="exibir = !exibir">Mostrar Mensagem</b-button>

		<transition name="fade" appear="">
		<b-alert variant="info" show v-show="exibir"> {{ msg }} </b-alert>
		</transition>
		<hr>
		<transition name="slide" appear="">
		<b-alert variant="info" show v-show="exibir"> {{ msg }} </b-alert>
		</transition>
		<hr>
		<transition 
		enter-active-class="animated bounce"
		leave-active-class="animated shake">
		<b-alert variant="info" show v-show="exibir"> {{ msg }} </b-alert>
		</transition>
		<hr>
		<b-select v-model="typeAnimate">
			<option value="fade">Fade</option>
			<option value="slide">Slide</option>
		</b-select>
		<transition :name="typeAnimate" mode="out-in">
		<b-alert variant="info" show v-if="exibir"  key="info"> {{ msg }} </b-alert>
		<b-alert variant="warning" show v-else key="warning"> {{ msg }} </b-alert>
		</transition>
		<hr>
		<b-button variant="danger"  @click="exibir2 = !exibir2">Alternar</b-button>
		<transition 
		:css="false"
		    @before-enter="beforeEnter"
		    @enter="enter"
		    @after-enter="afterEnter"
		    @enter-cancelled="enterCancelled"

		    @before-leave="beforeLeave"
		    @leave="leave"
		    @after-leave="afterLeave"
		    @leave-cancelled="leaveCancelled">
			<div v-if="exibir2" class="caixa"></div>
		</transition>
		<div class="mb-4">
			<b-btn variant="info" class="mr-3"  @click="componenteSelecionado = 'ComponentInfo'">Info</b-btn>
			<b-btn variant="warning"  @click="componenteSelecionado = 'ComponentWarning'">Advertência</b-btn>
		</div>
		<transition name="fade" mode="out-in">
		<component :is="componenteSelecionado"></component>

		</transition> -->  
		<b-button variant="danger" @click="adicionarAluno" class="mb-4"></b-button>
		<transition-group name="slide">
		<b-list-group v-for="(aluno, i) in alunos" :key="aluno">
		<b-list-group-item @click="removerAluno(i)"> {{ aluno }} </b-list-group-item>
		</b-list-group>
		</transition-group>

	</div>
</template>

<script>
import ComponentInfo from './components/ComponentInfo'
import ComponentWarning from './components/ComponentWarning'

export default {
	components: { ComponentInfo, ComponentWarning },
	data() {
		return {
			alunos: ['Maria', 'Jose', 'João', 'Tereza'],
			msg: 'Mensagem Enviada com Sucesso',
			exibir: false,
			exibir2: true,
			typeAnimate: 'fade',
			larguraBase: 0,
			componenteSelecionado: 'ComponentInfo'
		}
	},
	methods: {
		adicionarAluno(){
			const s = Math.random().toString(36).substring(2)
			this.alunos.push(s)
		},

		removerAluno(indice) { 
			this.alunos.splice(indice, 1)
		},
		
		beforeEnter(el) {
			this.larguraBase = 0
			el.style.width = `${this.larguraBase}px`
		},
		enter(el, done) {
			let rodada = 1
			const temporizador = setInterval(() => {
				const novaLargura = this.larguraBase + rodada * 10
				el.style.width = `${novaLargura}px`
				rodada++
				if(rodada > 30) {
					clearInterval(temporizador)
					done()
				}
			}, 20)
		},
		afterEnter(el) {
			console.log('afterEnter')
		},
		enterCancelled() {
			console.log('enterCancell')
		},
		beforeLeave(el) {
			this.larguraBase = 300
			el.style.width = `${this.larguraBase}px`
		},
		leave(el, done) {
			let rodada = 1
			const temporizador = setInterval(() => {
				const novaLargura = this.larguraBase + rodada * 10
				el.style.width = `${novaLargura}px`
				rodada++
				if(rodada > 30) {
					clearInterval(temporizador)
					done()
				}
			}, 20)
		},
		afterLeave(el) {
			console.log('afterleave')
		},
		leaveCancelled() {
			console.log('leaveCancell')
		},
	}

}
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
	font-size: 1.5rem;
}

.caixa {
	height: 200px;
	width: 300px;
	margin: 30px auto;
	background-color: rgb(204, 163, 241);
}

.fade-enter, .fade-leave-to{
opacity: 0;

}

.fade-enter-active, .fade-leave-active  {
transition: opacity 2s;
}

@keyframes slide-in {
	from { transform: translateY(40px); }
	to { transform: translateY(0); }
}

@keyframes slide-out {
	from { transform: translateY(0); }
	to { transform: translateY(40px); }
}

.slide-enter-active {
	animation: slide-in 2s ease;
	transition: opacity 2s;
}
.slide-leave-active {
	position: absolute;
	width: 100%;
	animation: slide-out 2s ease;
	transition: opacity 2s;
}
.slide-enter, .slide-leave-to{
opacity: 0;

}
.slide-move {
	transition: transform 1s;
}
</style>
