<template>
  <div id="user-delete">
    <p class="text-bold text-center">--- User Delete ---</p>
    <div class="border border-danger rounded">
      <div class="mx-3 my-4">
        <h6 class="text-danger">
          This action will delete this user from membership.
        </h6>
        <h6 class="text-info">
          <i class="fas fa-exclamation-triangle"></i>&ensp;Info, after being deleted you can still restore this user.<br>
        </h6>
        <button
          class="btn btn-outline-danger text-bold mt-2"
          @click="deleteUser"
        ><i class="fas fa-user-times"></i>&ensp;Delete User</button>
      </div>
    </div>
  </div>
</template>

<script>
import Swal from 'sweetalert2'
export default {
  name: 'UserDelete',
  props: {
    defaultReturn: String
  },
  methods: {
    deleteUser () {
      Swal.fire({
        title: 'Are you sure ?',
        text: 'Delete this user from membership',
        icon: 'question',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, Delete!',
        cancelButtonText: 'Cancel'
      }).then(async result => {
        if (result.value) {
          this.$axios.getCookies().then(() => {
            this.$axios.deleteAdminMenuUserDelete(this.appTimeNow, this.userCode).then(async res => {
              if (res.data.status === 'success') {
                await Swal.fire('Success', `${res.data.message}`, 'success')
                return this.$router.push({ name: this.defaultReturn })
              } else Swal.fire('Failed', `${res.data.message}`, 'error')
            })
          })
        }
      })
    }
  },
  data () {
    return {
      appTimeNow: new Date().getTime(),
      userCode: this.$route.params.code
    }
  }
}
</script>
