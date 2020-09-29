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
                >
                  <img
                    class="h-8 w-8 rounded-full"
                    src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=144&h=144&q=80"
                    alt=""
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
                    >Your Profile</a
                  >
                  <a
                    href="#"
                    class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 transition ease-in-out duration-150"
                    role="menuitem"
                    >Settings</a
                  >
                  <a
                    href="#"
                    class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 transition ease-in-out duration-150"
                    role="menuitem"
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
// var verifier = require("google-id-token-verifier");

window.handleGoogleOneTapResponse = function(resp) {
  console.log(resp)
  // verifier.verify(resp.credential, resp.clientId, function(err, tokenInfo) {
  //   console.log(err);
  //   console.log(tokenInfo);
  // });
  // vuecomponent.handleGoogleOneTapResponse(e);
}

export default {
  name: "App",
  components: {},
  data() {
    return {
      hasLoggedIn: false,
      isAccountMenuOpen: false,
      isLeftSidebarOpenMobile: false,
    }
  },

  methods: {
    handleGoogleOneTapResponse(e) {
      console.log(e)
    },
  },
}
</script>
