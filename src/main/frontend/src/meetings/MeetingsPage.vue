<template>
  <div>
    <new-meeting-form @added="addNewMeeting($event)"></new-meeting-form>

    <span v-if="meetings.length == 0">
               Brak zaplanowanych spotkań.
           </span>
    <h3 v-else>
      Zaplanowane zajęcia ({{ meetings.length }})
    </h3>

    <meetings-list :meetings="meetings"
                   :username="this.username"
                   @attend="addMeetingParticipant($event)"
                   @unattend="removeMeetingParticipant($event)"
                   @delete="deleteMeeting($event)"></meetings-list>
  </div>
</template>

<script>
    import NewMeetingForm from "./NewMeetingForm";
    import MeetingsList from "./MeetingsList";
    export default {
        components: {NewMeetingForm, MeetingsList},
        props: ['username'],
        data() {
            return {
                meetings: []
            };
        },
        methods: {
            addNewMeeting(meeting) {
                this.$http.post('meetings', meeting)
                    .then(response => {
                        this.meetings.push(response.body);
                    });
                this.getMeetings();
            },
            
            addMeetingParticipant(meeting) {
                this.$http.post('meetings/' + meeting.id + '/participants/', this.username).then(response => {
                        this.meetings = response.body;
                    })
                meeting.participants.push(this.username);
                 
                 this.getMeetings();
            },
            removeMeetingParticipant(meeting) {
            this.$http.delete('meetings/' + meeting.id + '/participants/' + this.username).then(response => {
            this.meetings = response.body;
                    })
                meeting.participants.splice(meeting.participants.indexOf(this.username), 1);
            this.getMeetings();
            },
            deleteMeeting(meeting) {
                this.meetings.splice(this.meetings.indexOf(meeting), 1);
                this.$http.delete('meetings/' + meeting.id);
                this.getMeetings();
                
            },          
            getMeetings() {
                this.$http.get('meetings')
                    .then(response => {
                        this.meetings = response.body;
                    })
            },
            getParticipants(meeting) {
                this.$http.get('meetings/' + meeting.id + '/participants')
                    .then(response => {
                        this.meetings = response.body;
                    })
            }
        },
        mounted() {
            this.$nextTick(this.getMeetings());
            
          
        },
    }
</script>