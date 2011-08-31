# Setting Up and Using Convergence

1. go to [convergence.io](http://www.convergence.io/)
2. install the Firefox add-on
3. profit!

## Okay, that was fast...

here's the dough: once you've installed the add-on, it takes over the validation of SSL certificates;
the traditional CAs don't matter anymore. instead, when you visit a website via `https`, this is what happens:

1. an SSL connection is established to the site as usual.
2. the add-on checks whether it has seen and verified this cert before. if so, your connection goes through.
3. if the cert is unknown, one or multiple *notaries* are asked to confirm its legitimacy.
   if they do, i.e. they have seen and verified it before, the cert is remembered and your connection goes through.

you can freely configure the list of notaries to trust.