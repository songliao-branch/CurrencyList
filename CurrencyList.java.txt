package com.songliao.app;

import com.songliao.app.MyApplication;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Currency;
import java.util.List;
import java.util.Locale;
import java.util.MissingResourceException;

/**
 * Created by liaosong on 2017/11/15.
 */

public class CurrencyList {

    private static CurrencyList singleton = new CurrencyList( );

    private CurrencyList() {
        init();
    }
    public static CurrencyList getInstance( ) {
        return singleton;
    }

    //unique, alphabetically sorted currency codes
    private List<String> uniqueCurCodes = new ArrayList<>();
    private List<String> displayList = new ArrayList<>();//e.g. Hong Kong Dollar(HKD)


    private void init(){
        List<String> codes = new ArrayList<>();
        for(Locale locale: Locale.getAvailableLocales()) {
            try {
                Currency currency = Currency.getInstance(locale);

                if (!codes.contains(currency.getCurrencyCode())) {
                    codes.add(currency.getCurrencyCode());
                }
            } catch (IllegalArgumentException | MissingResourceException e) {
            }
        }

        Collections.sort(codes);
        uniqueCurCodes = codes;

        Locale systemLocale = MyApplication.getApplicationContext().getResources().getConfiguration().locale;
        for(String code: uniqueCurCodes) {
            String name = Currency.getInstance(code).getDisplayName(systemLocale) + " (" + code + ")";
            displayList.add(name);
        }
    }

    public List<String> getCurrencyCodes() {
        return uniqueCurCodes;
    }

    public List<String> getCurrenciesForDisplay() {
        return displayList;
    }

}
