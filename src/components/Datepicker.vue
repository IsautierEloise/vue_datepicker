<style>
	.calendar-wrap
	{
		position: relative;
		top: 100%;
	}
</style>

<template>	
	<div class="calendar-wrap">
		<input type="text" v-model="date_formatted" @click="showCalendar" @focus="" readonly>
		<input type="hidden" :name="name"v-model="date_formatted" readonly>
		<calendar :statut="statut" v-model="date_raw" :dateProp="this.date" :displayedCalendar="visibleCalendar" :date.sync="date" @change="getDate" @cancel="hideCalendar" ></calendar>
	</div>
</template>

<script>
	//Imports
	import moment from 'moment';
	moment.locale('fr');
	import DatepickerCalendarComponent from './DatepickerCalendar.vue';

	//Variables
	const defaultFormat = 'YYYY-MM-DDTHH:mm';

	//Component
	export default
	{
		components: 
		{
			'calendar': DatepickerCalendarComponent,
		},
		props: 
		{
			value: { type: String, required: true },
			format: { type: String, default: defaultFormat },
			name: { type: String },
			statut: { 
				byDay: Boolean,
				byHalfDay: Boolean,
				byHour: Boolean,
				byMinute: Boolean,
			 },
			options: { type: String },
		},
		data () 
		{
			return {
				visibleCalendar: false,
				date: moment(this.value, defaultFormat),
			}
		},
		methods:
		{
			getDate: function (selectedDate)
			{
				this.date = selectedDate;
				this.hideCalendar();
			},
			showCalendar ()
			{
				this.visibleCalendar = true;
				setTimeout(() => document.addEventListener('click', this.hideCalendar), 0)
			},
			hideCalendar ()
			{
				this.visibleCalendar = false;
				document.removeEventListener('click', this.hideCalendar)
			},
		},
		computed: 
		{
			date_formatted () 
			{
				if(this.statut==='byDay')
				{
					return this.date.format('DD/MM/YYYY');
				}else
				{
					return this.date.format('DD/MM/YYYY HH:mm');
				}
			},
			date_raw () 
			{
				return this.date.format(defaultFormat)
			},
		},
	};
</script>