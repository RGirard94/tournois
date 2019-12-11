<template>
  <div class="tournaments">
    <header class="header">
      <h1>TOURNOIS</h1>
      <div class="tournament-info">
        <input type="text" class="new-tournament" placeholder="Nom du tournoi" @keyup.enter="addTournament" v-model="tournamentDetails.tournamentName">
        <input type="text" class="new-tournament" placeholder="Ajouter une date" @keyup.enter="addTournament" v-model="tournamentDetails.tournamentDate">
        <input type="text" class="new-tournament" placeholder="Ajouter une adresse" @keyup.enter="addTournament" v-model="tournamentDetails.tournamentAddress">
      </div>
    </header>
  </div>
  <div class="main">
    <ul class="tournament-list">
      <li class="tournament" v-for="tournament in tournamentsList">
        <div class="view">
          <input type="checkbox" class="toggle" v-model="tournament.showChoices" @click="resetOtherTournamentsInfo(tournament.name)">
          <label :class="{completed: tournament.SH_SD || tournament.DH_DD || tournament.DM}">{{ tournament.name }} {{ tournament.date }} {{ tournament.address }}</label>
          <div v-if="tournament.showChoices">
            <ul class="tournament-list">
              <li><input type="checkbox" v-model="tournament.SH_SD" class="toggle">
              <label>SH/SD</label></li>
              <li><input type="checkbox" v-model="tournament.DH_DD" class="toggle">
              <label>DH/DD</label></li>
              <li><input type="checkbox" v-model="tournament.DM" class="toggle">
              <label>DM</label></li>
            </ul>
          </div>
        </div>
      </li>
    </ul>
  </div>

</template>

<script>
export default {
  data () {
    return {
      tournamentsList: [],
      tournamentDetails: {tournamentName: '', tournamentDate: '', tournamentAddress: ''}
    }
  },

  methods: {
    addTournament () {
      if (this.tournamentDetails.tournamentName !== '' && this.tournamentDetails.tournamentDate !== '' && this.tournamentDetails.tournamentAddress !== '') {
        this.tournamentsList.push({name: this.tournamentDetails.tournamentName, date: this.tournamentDetails.tournamentDate, address: this.tournamentDetails.tournamentAddress, showChoices: false, SH_SD: false, DH_DD: false, DM: false})
        this.tournamentDetails.tournamentName = ''
        this.tournamentDetails.tournamentDate = ''
        this.tournamentDetails.tournamentAddress = ''
      }
    },

    resetOtherTournamentsInfo (t) {
      for (var i = 0; i < this.tournamentsList.length; i++) {
        if (this.tournamentsList[i].name !== t) {
          this.tournamentsList[i].showChoices = false
        }
      }
    },

    test () {
      console.log(this.tournamentsList)
    }
  },

  computed: {
    isRegistered () {
      return this.tournamentsList.filter(tournament => (tournament.SH_SD || tournament.DH_DD || tournament.DM))
    }
  }
}
</script>

<style>
html,
body {
	margin: 0;
	padding: 0;
}

body {
	font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
	line-height: 1.4em;
	background: #f5f5f5;
	color: #4d4d4d;
	min-width: 230px;
	max-width: 550px;
	margin: 0 auto;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	font-weight: 300;
}

:focus {
	outline: 0;
}

.hidden {
	display: none;
}

.inline {
  display: inline-block;
}

.tournaments {
	background: #fff;
	margin: 130px 0 40px 0;
	position: relative;
	box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2),
	            0 25px 50px 0 rgba(0, 0, 0, 0.1);
}

.tournaments input::-webkit-input-placeholder {
	font-style: italic;
	font-weight: 300;
	color: #e6e6e6;
}

.tournaments input::-moz-placeholder {
	font-style: italic;
	font-weight: 300;
	color: #e6e6e6;
}

.tournaments input::input-placeholder {
	font-style: italic;
	font-weight: 300;
	color: #e6e6e6;
}

.tournaments h1 {
	position: absolute;
	top: -155px;
	width: 100%;
	font-size: 100px;
	font-weight: 100;
	text-align: center;
	color: rgba(175, 47, 47, 0.15);
	-webkit-text-rendering: optimizeLegibility;
	-moz-text-rendering: optimizeLegibility;
	text-rendering: optimizeLegibility;
}

.new-tournament,
.edit {
	position: relative;
	margin: 0;
	width: 100%;
	font-size: 24px;
	font-family: inherit;
	font-weight: inherit;
	line-height: 1.4em;
	border: 0;
	color: inherit;
	padding: 6px;
	border: 1px solid #999;
	box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
	box-sizing: border-box;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}

.new-tournament {
	padding: 16px 16px 16px 60px;
	border: none;
	background: rgba(0, 0, 0, 0.003);
	box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
}

.tournament-list {
	margin: 0;
	padding: 0;
	list-style: none;
}

.tournament-list li {
	position: relative;
	font-size: 24px;
	border-bottom: 1px solid #ededed;
}

.tournament-list li:last-child {
	border-bottom: none;
}

.tournament-list li.editing {
	border-bottom: none;
	padding: 0;
}

.tournament-list li.editing .edit {
	display: block;
	width: 506px;
	padding: 12px 16px;
	margin: 0 0 0 43px;
}

.tournament-list li.editing .view {
	display: none;
}

.tournament-list li .toggle {
	text-align: center;
	width: 40px;
	/* auto, since non-WebKit browsers doesn't support input styling */
	height: auto;
	position: absolute;
	top: 0;
	bottom: 1;
	margin: auto 0;
	border: none; /* Mobile Safari */
	-webkit-appearance: none;
	appearance: none;
}

.tournament-list li .toggle {
	opacity: 0;
}

.tournament-list li .toggle + label {
	/*
		Firefox requires `#` to be escaped - https://bugzilla.mozilla.org/show_bug.cgi?id=922433
		IE and Edge requires *everything* to be escaped to render, so we do that instead of just the `#` - https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/7157459/
	*/
	background-image: url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23ededed%22%20stroke-width%3D%223%22/%3E%3C/svg%3E');
	background-repeat: no-repeat;
	background-position: center left;
}

.tournament-list li .toggle:checked + label {
	background-image: url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23bddad5%22%20stroke-width%3D%223%22/%3E%3Cpath%20fill%3D%22%235dc2af%22%20d%3D%22M72%2025L42%2071%2027%2056l-4%204%2020%2020%2034-52z%22/%3E%3C/svg%3E');
}

.tournament-list li label {
	word-break: break-all;
	padding: 15px 15px 15px 60px;
	display: block;
	line-height: 1.2;
	transition: color 0.4s;
}

.tournament-list label.completed {
	color: #3FFF33;
}



.tournament-list li .destroy {
	display: none;
	position: absolute;
	top: 0;
	right: 10px;
	bottom: 0;
	width: 40px;
	height: 40px;
	margin: auto 0;
	font-size: 30px;
	color: #cc9a9a;
	margin-bottom: 11px;
	transition: color 0.2s ease-out;
}

.tournament-list li .destroy:hover {
	color: #af5b5e;
}

.tournament-list li .destroy:after {
	content: '×';
}

.tournament-list li:hover .destroy {
	display: block;
}

.tournament-list li .edit {
	display: none;
}

.tournament-list li.editing:last-child {
	margin-bottom: -1px;
}

.main {
	position: relative;
	z-index: 2;
	border-top: 1px solid #e6e6e6;
}

.toggle-all {
	text-align: center;
	border: none; /* Mobile Safari */
	opacity: 0;
	position: absolute;
}

.toggle-all + label {
	width: 60px;
	height: 34px;
	font-size: 0;
	position: absolute;
	top: -52px;
	left: -13px;
	-webkit-transform: rotate(90deg);
	transform: rotate(90deg);
}

.toggle-all + label:before {
	content: '❯';
	font-size: 22px;
	color: #e6e6e6;
	padding: 10px 27px 10px 27px;
}

.toggle-all:checked + label:before {
	color: #737373;
}

/*
	Hack to remove background from Mobile Safari.
	Can't use it globally since it destroys checkboxes in Firefox
*/
@media screen and (-webkit-min-device-pixel-ratio:0) {
	.toggle-all,
	.tournament-list li .toggle {
		background: none;
	}

	.tournament-list li .toggle {
		height: 40px;
	}
}

@media (max-width: 430px) {
	.footer {
		height: 50px;
	}

	.filters {
		bottom: 10px;
	}
}

</style>
