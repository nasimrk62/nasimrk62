 public String save() {
        try {
            Boolean verifyCode = false;
            verifyCode = randomCode.equals(confirmCode);
            if (verifyCode) {
                if (!StringUtils.isEmpty(cardGroup)) {
                    accountOpenings = me.getMessageHandlerUtil().accountOpeningInfo(Integer.parseInt(branch), Integer.parseInt(accountType), Long.parseLong(me.getCif()), Integer.valueOf(cardGroup));
                } else {
                    accountOpenings = me.getMessageHandlerUtil().accountOpeningInfo(Integer.parseInt(branch), Integer.parseInt(accountType), Long.parseLong(me.getCif()), -1);
                }
                if (accountOpenings.getSuccess().equals("-104")) {
                    me.addGlobalErrorMessage("acc_oppening_error");
                    return "";
                }
                if (accountOpenings.getSuccess().equals("-105")) {
                    me.addGlobalErrorMessage("app_oppening_acc_problem");
                    return "";
                }
                if (!accountOpenings.getSuccess().equals("1")) {
                    me.addGlobalErrorMessage("account_opening_error_in_sp");
                    return null;
                }
//            if (!accountOpenings.getSuccess().equals("1")) {
//                me.addGlobalErrorMessage("account_opening_error_in_sp");
//                return "";
//            }
                if (accountOpenings.equals("")) {
                    me.addGlobalErrorMessage("account_opening_error");
                    return "";
                } else {
                    me.addGlobalInfoMessage("opening_account_result_ok");
                    return "account-opening-result";
                }
            } else {
                me.addGlobalErrorMessage("confirm_code_incorrect");
                return "";
            }
        } catch (Exception ex) {
            ex.printStackTrace();
            me.addGlobalErrorMessage("account_opening_error");
            return "";
        }
    }

.................................
 public SelectItem[] getStates() {
        List<SelectItem> selectItems = new ArrayList<SelectItem>();
        if (stateList != null) {
            for (StateInfo stateInfo : stateList) {
                selectItems.add(new SelectItem(stateInfo.getStateId(), stateInfo.getStateName()));
            }
        }
        return selectItems.toArray(new SelectItem[selectItems.size()]);
    }
