<template>
  <section>
    <h2>{{ teamName }}</h2>
    <ul>
      <user-item
        v-for="member in members"
        :key="member.id"
        :name="member.fullName"
        :role="member.role"
      ></user-item>
    </ul>
    <RouterLink to="/teams/t2">Go to team 2</RouterLink>
  </section>
</template>

<script>
import UserItem from '../users/UserItem.vue';

export default {
  inject: ['users', 'teams'],
  props: ['teamId'],
  components: {
    UserItem,
  },
  data() {
    return {
      teamName: 'Test',
      members: [],
    };
  },
  methods: {
    fetchTeamMembers(teamId) {
      const currentTeams = this.teams.find((team) => team.id === teamId);
      const memberIdsOfCurrentTeam = currentTeams.members;
      const membersListOfCurrentTeam = [];
      for (const memberId of memberIdsOfCurrentTeam) {
        const currentTeamUser = this.users.find((user) => user.id === memberId);
        membersListOfCurrentTeam.push(currentTeamUser);
      }
      this.members = membersListOfCurrentTeam;
      this.teamName = currentTeams.name;
    },
  },
  created() {
    this.fetchTeamMembers(this.teamId);
    console.log(this.$route.query);
  },
  watch: {
    teamId(newTeamId) {
      this.fetchTeamMembers(newTeamId);
    },
  },
};
</script>

<style scoped>
section {
  margin: 2rem auto;
  max-width: 40rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  border-radius: 12px;
}

h2 {
  margin: 0.5rem 0;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
