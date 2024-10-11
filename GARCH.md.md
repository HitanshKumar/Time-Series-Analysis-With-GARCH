
# 📈Time Series Analysis Using GARCH model📉

WHAT IS GARCH❓

➡️ GARCH refers to Generalized Autoregressive Conditional Heteroskedasticity, which is essentially a time series model used to model volatility.

➡️ It is a more sophisticated version of the ARCH(1) model and uses not only the volatility but also the previous levels/prices of the time series.

✨ I have shown the difference in their equations and how the GARCH model is better at handling bursts in the time series via a note.


## DEPLOYMENT⚒️

Following are the imp code snipits⬇️

🥇 Plotting the ACF and PACF plot to fing the best order for GARCH⤵️
```bash
plot_acf(df['returns']**2)
plot_pacf(df['returns']**2)
```
🥈 Implementing the garch model and computing AIC score⤵️
```bash
garch_model = arch.arch_model(df['returns'], vol='GARCH', p=1, q=1)
aic_model = garch_model.fit(disp='off')
bic_model = garch_model.fit(disp='off', options={'ic': 'bic'})

if aic_model.aic < bic_model.bic:
    best_model = aic_model
else:
    best_model = bic_model

print(best_model.summary())

```
🥉Taking the new values into account via a loop⤵️
```bash
 rolling_predictions = []
test_size = 243

for i in range(test_size):
    train = returns[:-(test_size-i)]
    model = arch_model(train, p=1, q=1)
    model_fit = model.fit(disp='off')
    pred = model_fit.forecast(horizon=1)
    rolling_predictions.append(np.sqrt(pred.variance.values[-1,:][0]))
```

## Conclusion🎯

-GARCH, although a good model that considers both the previous levels and volatility of a time series, cannot be effectively used to model financial time series in particular.