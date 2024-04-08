# SignalApp-Setup 2.55  

I was recently tasked with setting up one Signal Server, after a few days of reading through most the developing threads here, I was able to successfully set up Video Calls, Voice Calls, Messages and Login and Registering. We arrived at the final part of setting up CloudFront CDN and then an unfortunate situation occurred, so I do not have any documentation on this yet.

I was removed from the project due to a dispute between me and the boss but was left with this documentation and decided to publicly release it, as its public information just spread across the forums in sections. This means that the documentation only covers up to the server-side configuration and not client building, but these steps are fairly easy to cover.

If you’re prepared to help out, please fork the repo and fill in any necessary steps or procedures that you feel are important or missing :smiley:

# Steps:

- [Requirements](./requirements.md)
- [Setup Signal Server](./setup_signal_server.md)
- [Setup Nginx & LetsEncrypt](./setup_nginx_and_letsencrypt.md)
- [Setup Turn And Stun](./setup_turn_and_stun.md)
- [Setup Whisper Store](./setup_whisper_store.md)
- Common Issues
  - [Account Database Crawler (Removes the horrid warn log)](./fixing-1000's-errors-in-signal-server.md)
  - [Redis crashing](./redis-bug.md)


Contact Information:

Email: office@litespeed.me

Telegram: @LiteeeDev
