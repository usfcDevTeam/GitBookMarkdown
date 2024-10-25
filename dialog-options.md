# Dialog Options

### Confirm Action

Confirm before save:

```typescript
export class AbcXyzDialog extends Serenity.EntityDialog<AbcXyzRow, any> {
	...
	...
	
	protected getToolbarButtons(): Serenity.ToolButton[] {
		let btns = super.getToolbarButtons();

		var btnSave = Q.first(btns, x => x.cssClass == "save-and-close-button");
		var btnApply = Q.first(btns, x => x.cssClass == "apply-changes-button");

		var oldSaveClick = btnSave.onClick;
		var oldApplyClick = btnApply.onClick;

		btnSave.onClick = e => { this.confirmBeforeSave(oldSaveClick, e); };
		btnApply.onClick = e => { this.confirmBeforeSave(oldApplyClick, e); };

		return btns;
	}

	private confirmBeforeSave(oldEvt, e) {
		Q.confirm("Here is confirm message?", () => {
			oldEvt(e);
		});
	}
	
	...
}
```

### Validate Form

```typescript
constructor(container: JQuery) {
    super(container);

    this.form = new ChangePasswordForm(this.idPrefix);
    this.form.NewPassword.addValidationRule(this.uniqueName, e => {
        if (this.form.w('ConfirmPassword', Serenity.PasswordEditor).value.length < 7) {
            return Q.format(Q.text('Validation.MinRequiredPasswordLength'), 7);
        }
    });

    this.form.ConfirmPassword.addValidationRule(this.uniqueName, e => {
        if (this.form.ConfirmPassword.value !== this.form.NewPassword.value) {
            return Q.text('Validation.PasswordConfirm');
        }
    });

    this.byId('SubmitButton').click(e => {
        e.preventDefault();

        if (!this.validateForm()) {
            return;
        }

        var request = this.getSaveEntity();
        Q.serviceCall({
            url: Q.resolveUrl('~/Account/ChangePassword'),
            request: request,
            onSuccess: response => {
                Q.information(Q.text('Forms.Membership.ChangePassword.Success'), () => {
                    window.location.href = Q.resolveUrl('~/');
                });
            }
        });
    });
}
```
