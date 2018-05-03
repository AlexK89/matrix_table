<template>
	<div id="app">
		<div class="table">
			<div class="table__header">
				<div v-for="title in titles" class="table__header__title">
					<h5>{{ title }}</h5>
				</div>
			</div>
			<div class="table__body__row" v-for="process in dataArray">
				<div class="table__body__row__cell">
					<p>{{process.name}}</p>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.topics" class="topics">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.requirements" class="requirements">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.risks">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.controls">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.reminders">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<p v-if="process.lastAttestation">{{isoToDate(process.lastAttestation.date)}}</p>
				</div>
				<div class="table__body__row__cell">
					<p>{{process.failures}}</p>
				</div>
				<div class="table__body__row__cell">
					<p>{{process.issues}}</p>
				</div>
				<div class="table__body__row__cell">
					<p class="status" :style="{backgroundColor: process.status}">{{process.status}}</p>
				</div>
			</div>
		</div>

	</div>
</template>

<script>
	export default {
		name: 'app',
		data() {
			return {
				dataArray: [],
				titles: [
					'Processes',
					'Topics and objectives',
					'Requirements',
					'Risks',
					'Controls',
					'Reminders',
					'Last Attest',
					'Failures',
					'Issues',
					'Status'
				]
			}
		},
		mounted() {
			this.getData();
		},
		methods: {
			isoToDate(date) {
				let ourDate = new Date(date).toDateString();

				return ourDate.substr(ourDate.indexOf(' ') + 1);
			},
			getData() {
				//AJAx request
				axios
					.get('https://demo7244264.mockable.io/matrix')
					.then(response => {
						this.dataArray = response.data;
					})
					.catch((error) => {
						console.log(error);
						setTimeout(() => {
							this.getData();
						}, 2000);
					});
			}
		}
	}
</script>

<style lang="scss" scoped>
	#app {
		width: 90%;
		max-width: 1800px;
		margin: 5em auto 0;
		overflow-x: scroll;
		font-family: 'Roboto', sans-serif;
		box-shadow: 0 10px 20px rgba(0,0,0,0.19),
					0 6px 6px rgba(0,0,0,0.23);
	}

	.table {
		display: table;
		border-collapse: collapse;

		&__header {
			display: table-row;
			background-color: #303F9F;
			color: #ffffff;
			font-size: 1.3em;


			&__title {
				display: table-cell;
				vertical-align: middle;
				padding: 0.5em 1em;
				font-weight: bold;
			}
		}

		&__body {

			&__row {
				display: table-row;
				transition: .3s;
				color: #757575;

				&:hover {
					box-shadow: 0 14px 28px rgba(0,0,0,0.25), 0 10px 10px rgba(0,0,0,0.22);
					background-color: #FF4081;
					color: #ffffff;
				}

				&__cell {
					display: table-cell;
					min-width: 150px;
					padding: 2em 1em;
					vertical-align: middle;

					ul {
						padding: 0;

						.topics,
						.requirements {
							list-style: none;
						}
					}

					.status {
						color: #ffffff;
						text-align: center;
						padding: 0.5em;
						border-radius: 3px;
					}
				}
			}
		}
	}
</style>
