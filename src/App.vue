<template>
  <div class="h-screen flex overflow-hidden bg-gray-100">
    <div class="flex flex-col w-0 flex-1 overflow-hidden">
      <div class="relative z-10 flex-shrink-0 flex h-16 bg-white shadow">
        <div class="flex-1 px-4 flex justify-between">
          <div class="flex-1 flex">
            <div class="w-full flex md:ml-0"></div>
          </div>
          <div class="ml-4 flex items-center md:ml-6">
            <!-- Profile dropdown -->
            <div v-if="!hasLoggedIn" class="ml-3 relative">
              <div
                id="g_id_onload"
                data-client_id="602748836967-fl9qjuc1tojhk69si04cjp5aej8i85oq.apps.googleusercontent.com"
                data-callback="handleGoogleOneTapResponse"
                data-cancel_on_tap_outside="false"
              ></div>
            </div>
            <div v-if="hasLoggedIn" class="ml-3 relative">
              <div>
                <button
                  class="max-w-xs flex items-center text-sm rounded-full focus:outline-none focus:shadow-outline"
                  id="user-menu"
                  aria-label="User menu"
                  aria-haspopup="true"
                  @click="toggleDropDownMenu"
                >
                  <img
                    class="h-8 w-8 rounded-full"
                    :src="userImage"
                    :alt="userName"
                  />
                </button>
              </div>
              <!--
                Profile dropdown panel, show/hide based on dropdown state.

                Entering: "transition ease-out duration-100"
                  From: "transform opacity-0 scale-95"
                  To: "transform opacity-100 scale-100"
                Leaving: "transition ease-in duration-75"
                  From: "transform opacity-100 scale-100"
                  To: "transform opacity-0 scale-95"
              -->
              <div
                v-if="isAccountMenuOpen"
                class="origin-top-right absolute right-0 mt-2 w-48 rounded-md shadow-lg"
              >
                <div
                  class="py-1 rounded-md bg-white shadow-xs"
                  role="menu"
                  aria-orientation="vertical"
                  aria-labelledby="user-menu"
                >
                  <a
                    href="#"
                    class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 transition ease-in-out duration-150"
                    role="menuitem"
                    @click="signOut"
                    >Sign out</a
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const { OAuth2Client } = require("google-auth-library");

window.handleGoogleOneTapResponse = function(resp) {
  const client = new OAuth2Client(resp.clientId);
  async function verify() {
    const ticket = await client.verifyIdToken({
      idToken: resp.credential,
      audience: resp.clientId
    });
    const payload = ticket.getPayload();
    window.appRef.userLogin(payload);
  }
  verify().catch(console.error);
};

export default {
  name: "App",
  components: {},
  data() {
    return {
      hasLoggedIn: false,
      userImage: "",
      userName: "",
      isAccountMenuOpen: false,
      isLeftSidebarOpenMobile: false
    };
  },
  created() {
    window.appRef = this;
    this.userName = this.$cookie.get("userName");
    this.userImage = this.$cookie.get("userImage");
    this.hasLoggedIn = this.userName !== "";
  },

  methods: {
    userLogin(idTokenPayload) {
      console.log(idTokenPayload);
      this.userName = idTokenPayload.name;
      this.userImage = idTokenPayload.picture;
      this.$cookie.set("userName", this.userName, 7); // expire in 7 days
      this.$cookie.set("userImage", this.userImage, 7); // expire in 7 days
      this.hasLoggedIn = this.userName !== "";
    },
    toggleDropDownMenu() {
      this.isAccountMenuOpen = !this.isAccountMenuOpen;
    },
    signOut() {
      this.userLogin({ name: "", image: "" });
      document.location.reload();
    }
  }
};
</script>
