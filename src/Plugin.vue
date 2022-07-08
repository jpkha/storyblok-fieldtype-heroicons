<script>
export default {
  mixins: [window.Storyblok.plugin],
  template: `
    <div>
    <div class="uk-margin-bottom uk-text-center" v-if="model.icon">
      Current icon:
      <div>
        <span style="margin: 5px; height: 24px; width: 24px; display: inline-block;" v-html="getSvg(model)"></span>
        <small style="cursor:pointer;" @click="removeIcon">(remove)</small>
      </div>
    </div>
    <input class="uk-width-1-1 uk-margin-bottom" v-model="search" placeholder="Search icons"/>
    <span class="icon-link" @click="setIcon(icon)" :key="icon.icon + icon.type" v-for="icon in filteredIcons"
          style="margin: 5px; height: 24px; width: 24px; display: inline-block;">
      <span v-html="getSvg(icon)"></span>
  </span>
    </div>`,
  data() {
    return {
      search: '',
      iconsName: ["academic-cap", "adjustments", "annotation", "archive", "arrow-circle-down", "arrow-circle-left", "arrow-circle-right", "arrow-circle-up", "arrow-down", "arrow-left", "arrow-narrow-down", "arrow-narrow-left", "arrow-narrow-right", "arrow-narrow-up", "arrow-right", "arrow-sm-down", "arrow-sm-left", "arrow-sm-right", "arrow-sm-up", "arrow-up", "arrows-expand", "at-symbol", "backspace", "badge-check", "ban", "beaker", "bell", "book-open", "bookmark-alt", "bookmark", "briefcase", "cake", "calculator", "calendar", "camera", "cash", "chart-bar", "chart-pie", "chart-square-bar", "chat-alt-2", "chat-alt", "chat", "check-circle", "check", "chevron-double-down", "chevron-double-left", "chevron-double-right", "chevron-double-up", "chevron-down", "chevron-left", "chevron-right", "chevron-up", "chip", "clipboard-check", "clipboard-copy", "clipboard-list", "clipboard", "clock", "cloud-download", "cloud-upload", "cloud", "code", "cog", "collection", "color-swatch", "credit-card", "cube-transparent", "cube", "currency-bangladeshi", "currency-dollar", "currency-euro", "currency-pound", "currency-rupee", "currency-yen", "cursor-click", "database", "desktop-computer", "device-mobile", "device-tablet", "document-add", "document-download", "document-duplicate", "document-remove", "document-report", "document-search", "document-text", "document", "dots-circle-horizontal", "dots-horizontal", "dots-vertical", "download", "duplicate", "emoji-happy", "emoji-sad", "exclamation-circle", "exclamation", "external-link", "eye-off", "eye", "fast-forward", "film", "filter", "finger-print", "fire", "flag", "folder-add", "folder-download", "folder-open", "folder-remove", "folder", "gift", "globe-alt", "globe", "hand", "hashtag", "heart", "home", "identification", "inbox-in", "inbox", "information-circle", "key", "library", "light-bulb", "lightning-bolt", "link", "location-marker", "lock-closed", "lock-open", "login", "logout", "mail-open", "mail", "map", "menu-alt-1", "menu-alt-2", "menu-alt-3", "menu-alt-4", "menu", "microphone", "minus-circle", "minus-sm", "minus", "moon", "music-note", "newspaper", "office-building", "paper-airplane", "paper-clip", "pause", "pencil-alt", "pencil", "phone-incoming", "phone-missed-call", "phone-outgoing", "phone", "photograph", "play", "plus-circle", "plus-sm", "plus", "presentation-chart-bar", "presentation-chart-line", "printer", "puzzle", "qrcode", "question-mark-circle", "receipt-refund", "receipt-tax", "refresh", "reply", "rewind", "rss", "save-as", "save", "scale", "scissors", "search-circle", "search", "selector", "server", "share", "shield-check", "shield-exclamation", "shopping-bag", "shopping-cart", "sort-ascending", "sort-descending", "sparkles", "speakerphone", "star", "status-offline", "status-online", "stop", "sun", "support", "switch-horizontal", "switch-vertical", "table", "tag", "template", "terminal", "thumb-down", "thumb-up", "ticket", "translate", "trash", "trending-down", "trending-up", "truck", "upload", "user-add", "user-circle", "user-group", "user-remove", "user", "users", "variable", "video-camera", "view-boards", "view-grid-add", "view-grid", "view-list", "volume-off", "volume-up", "wifi", "x-circle", "x", "zoom-in", "zoom-out"],
      heroIconsOutline: undefined,
      heroIconsSolid: undefined
    }
  },
  computed: {
    filteredIcons() {
      let iconsName = this.iconsName.filter((icon) => {
        return icon.indexOf(this.search) > -1
      })

      let result = []
      iconsName.forEach((icon) => {
        result.push({icon: icon, type: 'heroicon-outline'})
        result.push({icon: icon, type: 'heroicon-solid'})
      })
      return result.slice(0, 25)
    }
  },
  methods: {
    getSvg(icon) {
      if (icon.type === 'heroicon-outline' && this.heroIconsOutline) {
        return `<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">${this.heroIconsOutline.icons[icon.icon]}</svg>`
      } else if (icon.type === 'heroicon-solid' && this.heroIconsSolid) {
        return `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">${this.heroIconsSolid.icons[icon.icon]}</svg>`
      } else {
        return ''
      }
    },
    initWith() {
      return {
        plugin: 'heroicon',
        icon: '',
        svgIcon: '',
        type: ''
      }
    },
    setIcon(icon) {
      this.model.icon = ''
      this.$nextTick(() => {
        this.model.icon = icon.icon
        this.model.svgIcon = this.getSvg(icon)
        this.model.type = icon.type
      });
    },
    removeIcon() {
      this.model.icon = ''
      this.$nextTick(() => {
        this.model.icon = ''
        this.model.svgIcon = ''
        this.model.type = ''
      });
    },
    pluginCreated() {
      this.$sb.getScript('https://cdn.jsdelivr.net/npm/hero-icon-js/hero-icon-outline.min.js', () => {
        // eslint-disable-next-line
        const heroIconsOutline = new HeroIconOutline();
        // eslint-disable-next-line
        this.heroIconsOutline = heroIconsOutline;
      });
      this.$sb.getScript('https://cdn.jsdelivr.net/npm/hero-icon-js/hero-icon-solid.js', () => {
        // eslint-disable-next-line
        const heroIconsSolid = new HeroIconSolid();
        // eslint-disable-next-line
        this.heroIconsSolid = heroIconsSolid;
      })
    }
  },
  watch: {
    'model': {
      handler: function (value) {
        this.$emit('changed-model', value);
      },
      deep: true
    }
  }
}
</script>
