#. Go to *Settings > Technical > Database Structure > Attachments* and select some files.
#. Go to *Actions > Download* and a zip file containing the selected files will be downloaded.

## For developer

You can reuse the `IrAttachmentActionDownloadMixin` on your
favorite models::

    from odoo import models


    class StockPicking(models.Model):
        _name = "stock.picking"
        _inherit = ["stock.picking", "ir.attachment.action_download"]


Then you can add an action button on list view line or on the action button
(when multiple lines are selected) to download all files::

    <odoo>
        <!--
            add a button on list view to download all attachement present
            on the given transfert
        -->
        <record id="vpicktree" model="ir.ui.view">
            <field name="inherit_id" ref="stock.vpicktree"/>
            <field name="name">stock.picking.tree download attachments</field>
            <field name="model">stock.picking</field>
            <field name="arch" type="xml">
                <field name="json_popover" position="after">
                    <button name="action_download_attachments"
                        type="object"
                        icon="fa-download"
                        string="Download attachment(s)"
                        aria-label="Download Proof documents"
                        class="float-right"/>
                </field>
            </field>
        </record>

        <!--
            Add "Download attachments" item in the Action menu when
            multiple records are selected
        -->
        <record id="action_download_picking_attachements" model="ir.actions.server">
            <field name="name">Download attachments</field>
            <field name="model_id" ref="stock.model_stock_picking"/>
            <field name="binding_model_id" ref="stock.model_stock_picking"/>
            <field name="binding_view_types">list</field>
            <field name="state">code</field>
            <field name="code">
                action = records.action_download_attachments()
            </field>
        </record>
    </odoo>


.. note::

    Even you will be able to generate a zip file with multiple document with the
    same name it's advice to overwrite `_compute_zip_file_name` to improve the
    name.
